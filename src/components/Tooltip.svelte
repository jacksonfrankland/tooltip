<script>
    export let element;
    import {scale} from 'svelte/transition';

    let enabled = false;
    let container;
    let tail;
    let spaceAbove;

    $: if (container && tail) {
        let elementBox = element.getBoundingClientRect();
        spaceAbove = elementBox.y - 24 > container.offsetHeight;
        let horizontalPositions = [
            {
                willFit: elementBox.x + elementBox.width / 2 > container.offsetWidth * .75 && window.innerWidth - elementBox.x + elementBox.width / 2 > container.offsetWidth * .75,
                container: `${elementBox.x + elementBox.width / 2 - container.offsetWidth / 2}px`,
                tail: `${container.clientWidth / 2 - 10}px`
            },
            {
                willFit: elementBox.x + elementBox.width / 2 > container.offsetWidth,
                container: `${elementBox.x + elementBox.width / 2 - container.offsetWidth}px`,
                tail: `${container.clientWidth - 40}px`
            },
            {
                willFit: window.innerWidth - elementBox.x + elementBox.width / 2 > container.offsetWidth,
                container: `${elementBox.x + elementBox.width / 2}px`,
                tail: '20px'
            },
        ]
        let horizontalPosition = horizontalPositions.find(position => position.willFit) ?? horizontalPositions[0];
        container.style.left = horizontalPosition.container;
        tail.style.left = horizontalPosition.tail;
        container.style.top = spaceAbove ? `${elementBox.y - 24 - container.offsetHeight}px` : `${elementBox.y + elementBox.height + 24}px`;
        tail.style.top = spaceAbove ? `${container.clientHeight - 1}px` : `${-tail.clientHeight}px`;
    }

    element.addEventListener('mouseover', () => enabled = true);
    window.addEventListener('touchstart', event => enabled = event.target == element);
    element.addEventListener('mouseleave', () => enabled = false);
    element.addEventListener('touchcancel', () => enabled = false);
    window.addEventListener('resize', () => enabled = false);
    window.addEventListener('scroll', () => enabled = false);

</script>

{#if enabled}
    <div bind:this={container} transition:scale={{duration: 200}}>
        <svg bind:this={tail} xmlns="http://www.w3.org/2000/svg" class:flipped={!spaceAbove} width="20" height="20" viewBox='0 0 1 1'>
            <path stroke="none" d="M 0 0 L .5 1 L 1 0" />
            <path fill="none" d="M 0 .15 L .5 1 L 1 .15" />
        </svg>
        <slot />
    </div>
{/if}

<style>
    div {
        border-radius: .5rem;
        padding: 1rem;
        border: solid;
        box-shadow: gray 0px 8px 24px;
        position: absolute;
    }
    svg {
        position: absolute;
        overflow: visible;
        fill: white;
        stroke: black;
        stroke-width: .125;
        stroke-linejoin: round;
        stroke-linecap: round;
    }
    .flipped {
        transform: scale(1, -1);
        -webkit-transform: scale(1, -1);
    }
</style>
