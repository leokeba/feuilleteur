<script lang="ts">
    import { type QuestionRequest } from "./types";
	import ErrorMessage from "./ErrorMessage.svelte";
	import Answer from "./Answer.svelte";
    import { BackendService, type GetBooksResponse } from "../client";

    export let books: GetBooksResponse;

    let question: QuestionRequest = {
        question: "",
        book: "",
        percentage: 100,
        answer: {
            text: "",
            documents: [],
            status: "idle",
            error: ""
        }
    }

    async function postQuestion() {
        try {
            question.answer = {
                text: "",
                documents: [],
                status: "processing",
                error: ""
            }
            const response = await BackendService.askQuestion({body:question});

            if (response.data?.answer) {
                question.answer.text = response.data.answer;
                question.answer.documents = response.data.documents;
                question.answer.status = "ok";
            }
            else {
                question.answer.error = JSON.stringify(response.error);
                question.answer.status = "error";
            }

        } catch (error) {
            question.answer.status = "error";
            question.answer.error = String(error);
        }
    }
</script>

{#if books}
    <section id="questions-form">
        <article>
            <h4>Ask Questions</h4>
            <hr>
            <form>
                <label for="book">
                    Book
                    <select name="book" aria-label="Select" required bind:value={question.book}>
                        <option selected disabled value="">Select a book</option>
                        {#each books as book}
                        <option value={book.uuid}>
                            {book.title}
                        </option>
                        {/each}
                    </select>
                </label>
                <label for="percentage">
                    Percentage
                    <input type="number" name="percentage" placeholder="Percentage" aria-label="Percentage" bind:value={question.percentage} min="0" max="100">
                </label>
                <label for="question">
                    Question
                <input type="search" name="question" placeholder="Your Question" aria-label="Search" bind:value={question.question} />
                </label>
                <button aria-busy={question.answer.status == "processing"} on:click={postQuestion} class="full-width">
                    {question.answer.status == "processing" ? "Processing" : "Ask Question"}
                </button>
            </form>
            <ErrorMessage message={question.answer.error}></ErrorMessage>
        </article>
        <Answer answer={question.answer}></Answer>
    </section>
{/if}

<style>
    .full-width {
        width: 100%;
    }
</style>