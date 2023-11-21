<script>
    import { useChat } from 'ai/svelte';

    const { input, handleSubmit, messages } = useChat();
</script>

<svelte:head>
    <title>Vision</title>
    <meta name="description" content="Svelte demo app" />
</svelte:head>

<section class="flex flex-col justify-between h-screen p-6 bg-gray-100">
    <header class="mb-4">
        <h1 class="text-3xl font-bold text-gray-800">useChat</h1>
    </header>
    <ul class="flex-1 mb-4 overflow-auto bg-white shadow rounded-lg p-4">
        {#each $messages as message}
            <li class="mb-2 break-words">
                <span class="font-semibold text-blue-600">{message.role}:</span>
                {message.content}
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
    /* Ensuring the chat does not exceed the viewport height */
    section {
        max-height: 100vh;
    }
</style>