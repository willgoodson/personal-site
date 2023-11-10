<script lang="ts">
    import { onMount } from 'svelte'
    // TYPING ANIMATION
    const texts = [
        {text: "BACK-END WEB DEVELOPER", color: "brown"},
        {text: "SITE RELIABILITY ENGINEER", color: "darkgreen"},
        {text: "DATABASE ADMINISTRATOR", color: "purple"},
    ]

    let typed_text = "";
    let typed_color = "";

    async function type_sentence(sentence:String, delay = 80) {
        const letters = sentence.split("");
        let i = 0;
        for (let i = 0; i < letters.length; i++) {
            await wait(delay);
            typed_text += letters[i];
        }
        return;
    }

    async function delete_sentence() {
        let length = typed_text.length
        while (length != 0) {
            typed_text = typed_text.slice(0, -1)
            length = typed_text.length
            await wait(100)
        }

    }

    function wait(ms:number) {
        return new Promise(resolve => setTimeout(resolve, ms))
    }

    onMount(async () => {
        let text_selector = 0;
        while (true) {
            typed_color = texts[text_selector].color
            await type_sentence(texts[text_selector].text)
            await wait(800)
            await delete_sentence()
            await wait(400)
            text_selector++;
            if (text_selector > texts.length -1) {
                text_selector = 0;
            }
        }

    })

</script>

<span style="color: {typed_color}">{typed_text}</span>
