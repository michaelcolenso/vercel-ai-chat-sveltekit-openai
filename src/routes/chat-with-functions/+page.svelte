<script lang="ts">
  import { useChat } from 'ai/svelte';
  import type { ChatRequest, FunctionCallHandler } from 'ai';
  import { nanoid } from 'ai';

  const functionCallHandler: FunctionCallHandler = async (
    chatMessages,
    functionCall
  ) => {
    // ... handle function calls as per your logic ...
  };

  const { messages, input, handleSubmit } = useChat({
    api: '/api/chat-with-functions',
    experimental_onFunctionCall: functionCallHandler
  });
</script>

<svelte:head>
  <title>Home</title>
  <meta name="description" content="Svelte demo app" />
</svelte:head>

<section class="flex flex-col justify-between h-screen p-6 bg-gray-100">
  <header class="mb-4">
    <h1 class="text-3xl font-bold text-gray-800">useChat</h1>
    <p class="mt-2 text-gray-600">
      This is a demo of the <code class="bg-gray-200 rounded p-1">useChat</code> hook. It uses the
      <code class="bg-gray-200 rounded p-1">experimental_onFunctionCall</code> option to handle function calls from
      the model.
    </p>
    <p class="text-gray-600">
      The available functions are: <code class="bg-gray-200 rounded p-1">get_current_weather</code>,
      <code class="bg-gray-200 rounded p-1">get_current_time</code>,
      and <code class="bg-gray-200 rounded p-1">eval_code_in_browser</code>.
    </p>
  </header>
  <!-- Chat messages list -->
<ul class="space-y-2 overflow-auto p-4">
  {#each $messages as message}
    <li class="{message.role === 'assistant' ? 'bg-gray-100' : ''} p-2 rounded">
      <p class="{message.role}">{message.content}</p>
      <!-- If the message is from the assistant and contains code -->
      {#if message.role === 'assistant' && message.content.includes('```')}
        <pre class="bg-gray-700 text-white text-xs p-2 overflow-x-auto rounded my-2">
          <code class="whitespace-pre">
            {@html message.content.replace(/```/g, '')}
          </code>
        </pre>
      {/if}
    </li>
  {/each}
</ul>

  
  <form on:submit={handleSubmit} class="flex rounded-lg shadow-lg overflow-hidden">
    <input
      bind:value={$input}
      class="flex-1 p-4 text-gray-800 border-0 focus:ring-2 focus:ring-blue-300 focus:outline-none"
      placeholder="Type your message here..."
    />
    <button
      type="submit"
      class="px-8 bg-blue-500 text-white hover:bg-blue-600 focus:outline-none focus:ring-2 focus:ring-blue-300 focus:ring-opacity-50 transition ease-in duration-200"
    >
      Send
    </button>
  </form>
</section>

<style>
  /* Add any additional custom styles if needed */
</style>
