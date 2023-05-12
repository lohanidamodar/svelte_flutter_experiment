<script>
    import { onMount } from "svelte";
    export let onFlutterAppLoaded;
    /**
     * @type any
     */
    let target;
    onMount(() => {
      if (window._flutter) {
        window._flutter.loader.loadEntrypoint({
          entrypointUrl: "/flutter/main.dart.js",
          onEntrypointLoaded: async (engineInitializer) => {
            let appRunner = await engineInitializer.initializeEngine({
              hostElement: target,
              assetBase: "/flutter/",
            });
            await appRunner.runApp();
          },
        });
  
        target.addEventListener("flutter-initialized", (event) => {
            onFlutterAppLoaded(event);
        });
      }
    });
  </script>
  
  <svelte:head>
    <script src="/flutter/flutter.js"></script>
  </svelte:head>
  
  <div class="flutter_target" bind:this={target} />
  
  <style>
    .flutter_target {
      width: 300px;
      height: 450px;
      background-color: #f2f2f2;
      border: 1px solid #000;
      margin-left: 20px;
      margin-top: 20px;
    }
  </style>
  