<script>
  export let sharetext = "",
    sharetitle = "",
    shareurl = location.href,
    shareimg = undefined;

  let isMobile = window.matchMedia("(max-width: 600px)").matches;

  let share;

  if (shareimg && navigator.canShare) {
    share = async function() {
      const response = await fetch(shareimg);
      const blob = await response.blob();
      const imageExtension = blob.type.replace("image/", "");
      const file = new File([blob], `image.${imageExtension}`, {
        type: blob.type
      });

      navigator.share({
        url: shareurl,
        title: sharetitle,
        text: sharetext,
        files: [file]
      });
    };
  } else {
    share = function() {
      navigator.share({
        url: shareurl,
        title: sharetitle,
        text: sharetext
      });
    };
  }

  function shareToFacebook(event) {
    event.preventDefault();
    window.open(
      "https://www.facebook.com/sharer.php?u=" + shareurl,
      "popup",
      "width=600,height=600"
    );
  }

  const getWindowOptions = function() {
    var width = 500;
    var height = 350;
    var left = window.innerWidth / 2 - width / 2;
    var top = window.innerHeight / 2 - height / 2;

    return [
      "resizable,scrollbars,status",
      "height=" + height,
      "width=" + width,
      "left=" + left,
      "top=" + top
    ].join();
  };

  function shareToTwitter(event) {
    let win = window.open(
      `https://twitter.com/intent/tweet?url=${shareurl}`,
      "TwitterWindow",
      getWindowOptions()
    );
  }

  function copy() {
    navigator.clipboard.writeText(shareurl);
  }
</script>

<style>
  div {
    display: flex;
    flex-wrap: wrap;
  }

  button {
    display: inline-flex;
    align-items: center;
    padding: 0 16px;
    height: 48px;
    background-color: #000;
    font-size: 16px;
    font-weight: 700;
    line-height: 24px;
    letter-spacing: 0.3px;
    color: #fff;
    margin-top: 4px;
    border: solid currentColor 2px;
  }

  button:not(:last-child) {
    margin-right: 4px;
  }

  button:hover,
  button:focus {
    outline: 0;
    color: #fcc31e;
  }

  svg {
    margin-right: 8px;
    fill: currentColor;
    width: 24px;
    height: 24px;
  }
</style>

<svelte:options tag="social-share" />
{#if window.navigator.share && isMobile}
  <button on:click={share} class="gg-c-share__button">
    <svg
      class="gg-c-share__icon"
      xmlns="http://www.w3.org/2000/svg"
      viewbox="0 0 24 24">
      <path
        d="M17.9 14.5c-1 0-1.9.5-2.5 1.2-2.2-1.3-4.4-2.3-6.6-3.4v-.4-.2c2.3-1.1
        4.5-2.3 6.9-3.6.5.4 1.1.7 1.7.8.2.1.3.1.4.1 1.7 0 3-1.4 3.1-3
        0-1.7-1.3-3.1-3-3.1s-3.1 1.3-3.1 3c0 .4.1.7.2 1l-6.6 3.4C7.8 9.5 6.9 9
        5.8 9c-1.7 0-3.1 1.4-3.1 3.1 0 1.7 1.4 3.1 3.1 3.1 1.1 0 2-.6
        2.6-1.5l6.5 3.4c0 .1-.1.3-.1.4-.1 1.7 1.3 3.1 3 3.2 1.7 0 3.1-1.3
        3.1-3s-1.3-3.2-3-3.2zm-.1-10.3c2.2 0 2.2 3.3 0 3.4-2.1 0-2.2-3.4
        0-3.4zM7.1 13c-.3.4-.7.8-1.3.8-2.1-.1-2.1-3.4 0-3.4.5 0 .8.2
        1.1.5.3.3.5.7.5 1.2 0 .3-.1.6-.3.9zm10.7 6.3c-2.2-.1-2.2-3.4 0-3.4s2.2
        3.3 0 3.4z"
        fill="currentColor" />
    </svg>
    Share
  </button>
{:else}
  <div>

    <button on:click={shareToFacebook} class="gg-c-share__button">
      <svg
        class="gg-c-share__icon"
        xmlns="http://www.w3.org/2000/svg"
        viewbox="0 0 64.8 124.2">
        <path
          d="M42.9,68.4c0-.3,0-.3.3-.3H61.5a.29.29,0,0,0,.3-.3c.3-2.7,2.7-21.9,2.7-22.2H43.2c-.3,0-.3,0-.3-.3v-15c0-1.2.3-3,.3-3.9.3-2.7,2.7-4.2,5.1-5.1a29.11,29.11,0,0,1,5.1-.3H64.8V.9C63.9.9,49.5,0,46.5,0A31.82,31.82,0,0,0,32.7,3c-5.7,3-9.3,7.2-12,13.2-.9,2.7-1.2,4.8-1.8,7.2a44.31,44.31,0,0,0-.3,6.3v15c0,.3,0,.3-.3.3H0V67.2H18.3c.3,0,.3,0,.3.3v56.7h24C42.9,124.2,42.9,85.8,42.9,68.4Z" />
      </svg>
      Share
    </button>
    <button on:click={shareToTwitter} class="gg-c-share__button">
      <svg
        class="gg-c-share__icon"
        xmlns="http://www.w3.org/2000/svg"
        viewbox="0 0 111.9 90.3">
        <title>Twitter</title>
        <path
          d="M111.9,10.8a40.67,40.67,0,0,1-13.2,3.3,23.43,23.43,0,0,0,9.9-12.3A39.61,39.61,0,0,1,93.9,7.5,22.73,22.73,0,0,0,54.3,22.8a29.11,29.11,0,0,0,.3,5.1A64.26,64.26,0,0,1,7.5,3.9a21.26,21.26,0,0,0-3,11.7A22.17,22.17,0,0,0,14.7,34.5a19.38,19.38,0,0,1-10.2-3v.3A22.55,22.55,0,0,0,22.8,54a36,36,0,0,1-6,.9,19.81,19.81,0,0,1-4.2-.3c3,9,11.1,15.3,21.3,15.9A46.64,46.64,0,0,1,5.7,80.4,36.31,36.31,0,0,1,0,80.1,65.67,65.67,0,0,0,34.8,90.3C77.1,90,99.9,55.2,99.9,25.2v-3A43.46,43.46,0,0,0,111.9,10.8Z" />
      </svg>
      Tweet
    </button>
    {#if window.navigator.clipboard}
      <button on:click={copy} class="gg-c-share__button">
        <svg
          class="gg-c-share__icon"
          xmlns="http://www.w3.org/2000/svg"
          viewbox="0 0 24 24">
          <path
            d="M15.7 3.3C14.3 4 13 5.5 12 6.7c-.3.3-.3.8 0 1.1.2.2.8.2 1-.1.8-.8
            1.6-1.7 2.4-2.4.2-.2.5-.4.7-.6.1-.1.4-.2.6-.3.1 0 .2-.1.3-.1h.3c.1 0
            .2.1.2.1h.1c.4.4.7.6.9.8.5.4.9.9 1.3 1.4.3.4.2 1 0 1.4-1.2 1.9-3.3
            3.3-5 4.7-1.4 1.1-2.6.6-4-.5-.8-.6-1.6.8-.8 1.4 1.8 1.4 3.7 1.9
            5.5.5 1.1-.9 2.1-1.8 3.2-2.8 1-1 2.5-2.4 2.6-4 .3-2.6-3.2-5.2-5.6-4z" />
          <path
            d="M13.9 10.4c-1.2-1.4-3-1.7-4.6-1.1-.7.4-1.3.9-2 1.4-1.5 1.5-3.6
            3-4.4 4.9-1 2.5 1.2 5.5 3.8 5.7 2.4.1 4.3-2.6
            5.9-4.1.7-.7-.3-1.8-1.1-1.1-1 1.1-2.1 2.1-3.3
            3.2-.9.7-2.3.5-2.9-.5-1-.8-1.2-2.4-.5-3.4l.1-.1c.9-1 1.9-2 3.1-2.9
            1.4-1.4 3.1-2.8 4.9-.8.5.7 1.7-.4 1-1.2z" />
        </svg>
        Copy link
      </button>
    {/if}
  </div>
{/if}
