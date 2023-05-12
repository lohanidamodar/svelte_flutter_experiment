<script>
  import FlutterApp from "./flutterApp.svelte";
  /** @type any */
  let flutterState;
  let count;
  let text;
  const onFlutterAppLoaded = (event ) => {
    console.log(event.detail);
    flutterState = event.detail;
    count = flutterState.getClicks();
    flutterState.onClicksChanged(() => { onCounterChanged() });
    flutterState.onTextChanged(() => { onTextChanged() });
  }

  const onCounterSet = (event) => {
    let clicks = parseInt((event.target).value, 10) || 0;
    flutterState.setClicks(clicks);
  }

  const onTextSet = (event) => {
    flutterState.setText((event.target).value || '');
  }

  // I need to force a change detection here. When clicking on the "Decrement"
  // button, everything works fine, but clicking on Flutter doesn't trigger a
  // repaint (even though this method is called)
  const onCounterChanged = () => {
    count = flutterState.getClicks();
  }

  const onTextChanged = () => {
    text = flutterState.getText();
  }

  let screen = 'counter';
  const screenChanged = (event ) => {
    flutterState.setScreen(screen);
  }
</script>


<h1>Welcome to SvelteKit</h1>
<p>
  Visit <a href="https://kit.svelte.dev">kit.svelte.dev</a> to read the documentation
</p>
<select name="Screen" id="screen" bind:value={screen} on:change={screenChanged}>
  <option value="counter">counter</option>
  <option value="text">TextField</option>
  <option value="dash">Custom App</option>
</select>

{#if screen === 'counter'}
<p>{count}</p>
{:else}
<input type="text" bind:value={text} on:keyup={() => flutterState.setText(text)} on:change={() => flutterState.setText(text)} />
{/if}


<FlutterApp onFlutterAppLoaded={onFlutterAppLoaded} />