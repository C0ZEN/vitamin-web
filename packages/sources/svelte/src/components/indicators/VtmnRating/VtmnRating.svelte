<script>
  import VtmnIcon from '../../../guidelines/iconography/VtmnIcon/VtmnIcon.svelte';
  import { cn } from '../../../utils/classnames';
  import { VTMN_RATING_SIZE } from './enum';
  import { computeRatingFill } from './vtmnRating.util';

  /**
   * @type {string} name used on interactive mode to name the inputs
   * @required
   */
  export let name;

  /**
   * @type {boolean} use the emphasis mode. Only if readonly is true.
   * @default false
   */
  export let emphasis = false;

  /**
   * @type {string} size of the component
   * @default medium
   */
  export let size = VTMN_RATING_SIZE.MEDIUM;

  /**
   * @type {boolean} disable the component
   * @default false
   */
  export let disabled = false;

  /**
   * @type {boolean} is readonly component
   * @default false
   */
  export let readonly = false;

  /**
   * @type {boolean} enable the compact mode. Only if readonly is true.
   * @default false
   */
  export let compact = false;

  /**
   * @type {number} rating to display on the component
   * @requires
   */
  export let value;

  /**
   * @type {boolean} display the rating value. Only if readonly is true.
   * default false
   */
  export let showValue = false;

  /**
   * @type {number} Comments displayed after the rating. Only if readonly is true.
   */
  export let comments = undefined;

  let className = undefined;
  /**
   * @type {string} Custom classes to apply to the component.
   */
  export { className as class };

  $: componentClass = cn(
    'vtmn-rating',
    size && `vtmn-rating_size--${size}`,
    emphasis && 'vtmn-rating_variant--brand',
    className,
  );
  $: starsCnt = compact && readonly ? 1 : 5;
</script>

<div class={componentClass} aria-disabled={disabled} {...$$restProps}>
  {#if readonly}
    {#each Array(starsCnt) as _, index}
      {@const position = index + 1}
      <VtmnIcon
        value={`star-${computeRatingFill(compact, position, value)}`}
        role="presentation"
      />
    {/each}
    {#if showValue}
      <span class="vtmn-rating_comment--primary">
        {value}/5
      </span>
    {/if}
    {#if comments}
      <span class="vtmn-rating_comment--secondary">
        {comments}
      </span>
    {/if}
  {:else}
    <div
      class="vtmn-rating--interactive"
      role="radiogroup"
      data-rating={value}
      {...$$restProps}
    >
      {#each Array(starsCnt) as _, index}
        {@const position = index + 1}
        <input
          type="radio"
          bind:group={value}
          {name}
          value={position}
          id={`${name}-${position}`}
          aria-label={`${position}/5`}
          {disabled}
        />
        <label for={`${name}-${position}`} />
      {/each}
    </div>
  {/if}
</div>

<style>
  @import '@vtmn/css-rating';
</style>
