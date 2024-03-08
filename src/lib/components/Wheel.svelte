<script>
    import { onMount } from "svelte";

    export let onRevolve = undefined;
    export let onSnap = undefined;
    export let onDistanceUpdated = undefined;
    export let fixedWidth = undefined;
    export let maxWidth = undefined;
    export let visibleOverflow = false;
    export let speedFactor = 1.0;

    export function getElements() {
        return wheelEl.children;
    }

    export function advance(direction) {
        if (direction < 0)
            snap.set(getElementRect(wheelEl).width / getElements().length, 0.1);
        else
            snap.set(
                -getElementRect(wheelEl).width / getElements().length,
                0.1
            );
    }

    const minSpeed = 8; //min impulse to be considered
    const maxSpeed = 20 * speedFactor; //used for limiting impulse
    const disaccel = 0.99;

    //to control gesture-based controls
    let clicked = false;
    let lastMousePos;
    let impulse = 1;
    let wheelPos = 0;

    let isBeeingResized = false;

    let moved = 0;
    let initialIndex;
    let globalIndex = 0;

    let wheelEl;
    let holderEl;

    // information about centering ('snaping') the wheel
    let snap = {
        state: 0, //0-not moving, 1-going to offshoot 2-going to final destination
        direction: 1,
        target: 0,
        offTarget: 0,
        speed: 6 * speedFactor,
        offSpeed: 10 * speedFactor,
        offshoot: 0.6 / speedFactor,
        set: function (t, offshoot) {
            if (offshoot == undefined) offshoot = this.offshoot;
            if (Math.abs(t) > 1) {
                this.offTarget = (t * (1.0 + offshoot)).clamp(t - 40, t + 40);
                this.target = t - this.offTarget;
                this.state = 1;
            }
        },
    };

    $: wheelEl && wheelEl.style.setProperty("left", `${wheelPos}px`);

    Number.prototype.clamp = function (min, max) {
        return Math.min(Math.max(this, min), max);
    };

    function getElementRect(element) {
        let rect = element.getBoundingClientRect();
        rect.x = rect.left + window.scrollX;
        rect.y = rect.top + window.scrollY;
        return rect;
    }

    function getDistances() {
        const holderRect = getElementRect(holderEl);
        const wheelWidth = getElementRect(wheelEl).width;
        const wrapperCenter = holderRect.x + holderRect.width / 2;
        const blockWidth = wheelWidth / getElements().length;

        let distances = [];
        for (let i = 0; i < getElements().length; i++) {
            const distance =
                wrapperCenter -
                (wheelPos + holderRect.x + blockWidth * (i + 0.5));
            distances.push(distance);
        }
        return distances;
    }
    export function getCenterIndex(distances) {
        if (distances == undefined) {
            distances = getDistances();
        }

        return distances.reduce(
            (index, value, i, array) =>
                Math.abs(value) < Math.abs(array[index]) ? i : index,
            0
        );
    }

    function moveWheel(amount) {
        // console.log('moving amount:',amount);
        let newPos = wheelPos + amount;
        let wheelRect = getElementRect(wheelEl);
        let holderRect = getElementRect(holderEl);
        let blockWidth = wheelRect.width / getElements().length;
        if (onRevolve == undefined) {
            newPos = newPos.clamp(
                -wheelRect.width +
                    holderRect.width -
                    (holderRect.width - blockWidth) / 2,
                (holderRect.width - blockWidth) / 2
            );
        } else {
            if (newPos > 0) {
                //move last child to fist place
                let lastEl = wheelEl.lastElementChild;
                wheelEl.insertBefore(lastEl, wheelEl.firstChild);
                newPos -= blockWidth;
                moved--;

                onRevolve(
                    lastEl,
                    moved - Math.floor(wheelEl.children.length / 2)
                );
            } else if (
                wheelRect.width + newPos <
                holderRect.width + blockWidth
            ) {
                //move first child to last place
                let firstEl = wheelEl.firstElementChild;
                wheelEl.appendChild(firstEl);
                newPos += blockWidth;
                moved++;
                onRevolve(
                    firstEl,
                    moved + Math.floor(wheelEl.children.length / 2)
                );
            }
        }

        wheelPos = newPos;

        let distances = getDistances();

        globalIndex = getCenterIndex(distances) + moved - initialIndex;

        if (onDistanceUpdated != undefined) {
            const wheelWidth = getElementRect(wheelEl).width;
            for (let i = 0; i < wheelEl.children.length; i++) {
                onDistanceUpdated(
                    wheelEl.children[i],
                    Math.abs(distances[i] / wheelWidth)
                );
            }
        }
    }

    onMount(() => {
        if(visibleOverflow){
            holderEl.style.overflow = "visible";
        }
        initialIndex = getCenterIndex();
        new ResizeObserver(() => {
            isBeeingResized = true;
            if (onRevolve != undefined) {
                let holderRect = getElementRect(holderEl);
                let blockRect = getElementRect(wheelEl.children[0]);

                holderEl.style.setProperty(
                    "max-width",
                    `${blockRect.width * (wheelEl.children.length - 1)}px`
                );
                wheelEl.style.setProperty(
                    "min-width",
                    `${blockRect.width * 2 + holderRect.width}px`
                );
            }
            if (fixedWidth != undefined) {
                let blockWidth = fixedWidth * getElementRect(holderEl).width;
                if(maxWidth != undefined)
                    blockWidth = blockWidth.clamp(0, maxWidth);
                for (let element of getElements()) {
                    element.style.setProperty("width", `${blockWidth}px`);
                }
            }
        }).observe(holderEl);

        function update() {
            let holderRect = getElementRect(holderEl);
            let wheelRect = getElementRect(wheelEl);
            let blockWidth =
                getElementRect(wheelEl).width / getElements().length;

            const wrapperCenter = holderRect.x + holderRect.width / 2;

            let distances = [];
            for (let i = 0; i < getElements().length; i++) {
                const distance =
                    wrapperCenter -
                    (wheelPos + holderRect.x + blockWidth * (i + 0.5));
                distances.push(distance);
            }
            const centerIndex = distances.reduce(
                (index, value, i, array) =>
                    Math.abs(value) < Math.abs(array[index]) ? i : index,
                0
            );

            const distanceToCenter =
                wrapperCenter -
                (wheelPos + holderRect.x + blockWidth * (centerIndex + 0.5));

            if (isBeeingResized) {
                isBeeingResized = false;
                //keep wheel centered
                snap.set(distanceToCenter, 0.0);
            }
            if (!clicked) {
                if (snap.offTarget != 0) {
                    let amount = snap.offTarget.clamp(
                        -snap.offSpeed,
                        snap.offSpeed
                    );
                    snap.offTarget -= amount;
                    // console.log('snap move');
                    moveWheel(amount);
                } else if (snap.target != 0) {
                    let amount = snap.target.clamp(-snap.speed, snap.speed);
                    snap.target -= amount;
                    // console.log('snap move');
                    moveWheel(amount);
                    if (onSnap != undefined && snap.target == 0) {
                        onSnap(globalIndex);
                    }
                } else {
                    //limit impulse
                    impulse = Math.min(maxSpeed, Math.max(-maxSpeed, impulse));

                    //if is slow enough
                    if (Math.abs(impulse) < minSpeed) {
                        impulse = 0;
                        if (
                            centerIndex == 0 ||
                            centerIndex == getElements().length - 1
                        ) {
                            snap.set(distanceToCenter, 0.0);
                        } else snap.set(distanceToCenter);
                    } else {
                        impulse *= disaccel;
                        moveWheel(impulse);
                    }
                }
            }
            requestAnimationFrame(update);
        }
        moveWheel(0);
        update();
    });
</script>

<div bind:this={holderEl} id="holder">
    <!-- WheelEl -->
    <div
        role="slider"
        tabindex="0"
        aria-valuenow="0"
        id="wheel"
        aria-label="wheel"
        bind:this={wheelEl}
        on:touchstart={(e) => {
            lastMousePos = e.touches[0].clientX;
            clicked = true;
            snap.state = 0;
        }}
        on:touchend={() => {
            clicked = false;
        }}
        on:touchmove={(e) => {
            if (clicked) {
                impulse = e.touches[0].clientX - lastMousePos;
                moveWheel(impulse);
                lastMousePos = e.touches[0].clientX;
            }
        }}
        on:mousedown={(e) => {
            lastMousePos = e.clientX;
            clicked = true;
            snap.state = 0;
        }}
        on:mouseup={() => {
            clicked = false;
        }}
        on:mousemove={(e) => {
            if (clicked) {
                impulse = e.clientX - lastMousePos;
                moveWheel(impulse);
                lastMousePos = e.clientX;
            }
        }}
        on:mouseleave={() => {
            clicked = false;
        }}
        on:keyup={(e) => {
            if (e.key == "ArrowLeft") {
                snap.set(
                    getElementRect(wheelEl).width / getElements().length,
                    0.1
                );
            } else if (e.key == "ArrowRight") {
                snap.set(
                    -getElementRect(wheelEl).width / getElements().length,
                    0.1
                );
            }
        }}
    >
        <slot />
    </div>
</div>

<style>
    #holder {
        width: 100%;
        height: 100%;
        overflow: hidden;
    }
    #holder * {
        user-select: none;
    }
    #wheel {
        height: 100%;
        display: inline-flex;
        position: relative;
    }
</style>