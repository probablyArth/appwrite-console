<script lang="ts">
    import type { Action } from 'svelte/action';

    export let isSticky = false;

    const hasOverflow: Action<HTMLDivElement, (value: boolean) => void> = (node, callback) => {
        const observer = new ResizeObserver((entries) => {
            for (const entry of entries) {
                callback(entry.contentRect.width < entry.target.scrollWidth);
            }
        });

        observer.observe(node);

        return {
            destroy() {
                observer.disconnect();
            }
        };
    };

    let isOverflowing = false;
</script>

<div class="table-with-scroll u-margin-block-start-32" data-private>
    <div class="table-wrapper" use:hasOverflow={(v) => (isOverflowing = v)}>
        <table class="table" class:is-sticky-scroll={isSticky && isOverflowing}>
            <slot />
        </table>
    </div>
</div>
