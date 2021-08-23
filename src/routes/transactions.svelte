<script context="module">
    export async function preload({params}, {token}) {
        if (!token) {
            this.redirect(302, `/login`);
        }
    }
</script>

<script>
    import {goto} from '@sapper/app';
    import {post} from 'utils.js';
    let accountFrom = '';
    let accountTo = '';
    let amount = '';
    let explanation = '';
    let error = null;
    async function submit(event) {
        const response = await post(`auth/transactions`, {accountFrom, accountTo, amount, explanation});
        error = response.error;
        if (!response.error) {
            goto('/overview');
        }
    }
</script>

<svelte:head>
    <title>Transaction</title>
</svelte:head>

<div class="auth-page">
    <div class="container page">
        <div class="row">
            <div class="col-md-6 offset-md-3 col-xs-12">
                <h1 class="text-xs-center">Transaction</h1>

                {#if error}
                    <div class="alert alert-danger" role="alert">{error}</div>
                {/if}

                <form on:submit|preventDefault={submit}>
                    <fieldset class="form-group">
                        <input class="form-control form-control-lg" type="text" required placeholder="Account from"
                               bind:value={accountFrom}>
                    </fieldset>
                    <fieldset class="form-group">
                        <input class="form-control form-control-lg" type="text" required placeholder="Account to"
                               bind:value={accountTo}>
                    </fieldset>
                    <fieldset class="form-group">
                        <input class="form-control form-control-lg" type="number" required placeholder="Amount"
                               bind:value={amount}>
                    </fieldset>
                    <fieldset class="form-group">
                        <input class="form-control form-control-lg" type="text" required placeholder="Explanation"
                               bind:value={explanation}>
                    </fieldset>
                    <button class="btn btn-lg btn-primary pull-xs-right">
                        Send
                    </button>
                </form>
            </div>
        </div>
    </div>
</div>