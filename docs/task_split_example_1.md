# Example #1 of task split
__Goal__: Summarize, translate and email text

Keep in mind that only functions are used for further processing, steps are ignored.
__Genrated plan__
```
task: Summarize, translate and email text
inputs:
    - name: input_text
        value: "This is a sample text that needs to be summarized, translated and emailed."
functions: 
    - name: summarize_text
        description: This function takes a text as input and returns a summarized version of it. It uses a text summarization algorithm to achieve this.
        verification: Check if the output text is a shorter version of the input text and retains the main points.
        parameters:
            - name: text
            description: The text to be summarized
        outputs:
            - description: The summarized text

    - name: translate_text
        description: This function takes a text and a language code as input and returns the translated text. It uses a translation API to achieve this.
        verification: Check if the output text is in the specified language and retains the meaning of the input text.
        parameters:
            - name: text
            description: The text to be translated
            - name: language_code
            description: The language code for the language to translate the text into
        outputs:
            - description: The translated text

    - name: send_email
        description: This function takes an email address, a subject, and a body as input and sends an email. It uses an email sending API to achieve this.
        verification: Check if the email was sent successfully to the specified email address with the specified subject and body.
        parameters:
            - name: email_address
            description: The email address to send the email to
            - name: subject
            description: The subject of the email
            - name: body  
            description: The body of the email
        outputs:
            - description: Confirmation of email sent

steps:
    - name: Summarize the text
    reason: To shorten the text while retaining the main points
    function: summarize_text
    inputs:
        - name: text
            value: $input_text
    output:
        set_context_variable: summarized_text

    - name: Translate the text
    reason: To convert the text into French
    function: translate_text
    inputs:
        - name: text
            value: $summarized_text
        - name: language_code
            value: "fr"
    output:
        set_context_variable: translated_text

    - name: Email the text
    reason: To send the translated text to John Doe
    function: send_email
    inputs:
        - name: email_address
            value: "johndoe@example.com"
        - name: subject
            value: "Translated Text"
        - name: body
            value: $translated_text
    output:
        append_to_result: email_confirmation
```