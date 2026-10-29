# ChatGPT app submission

Source for the ChatGPT app submission form. Update it with the MCP server's tools.

- `chatgpt-app-submission.json`: tool annotations with justifications, test
  cases and negative test cases, in OpenAI's
  [import format](https://developers.openai.com/plugins/schemas/chatgpt-app-submission.v1.json).
  Import it in the form.

## Release notes (1.1.0)

- Agent skills for exercises, 3D animations, workouts, programs and questionnaires.
- Edits are saved as drafts the coach applies in Propusher; new items are saved straight away.
- Publish a pending draft from the chat when the coach asks.

## Demo video

The form takes a URL. Record in ChatGPT, signed in as the review account:

1. Connect Propusher (OAuth sign-in).
2. "Which hamstring exercises do I have in Propusher?"
3. "Build a 45-minute lower-body strength workout in Propusher from my exercises." Open the link in Propusher.
4. "In my Propusher workout "Upper Body A", change the bench press to 4 sets of 6." Show the draft in the editor.
5. "Publish the draft changes on my "Upper Body A" workout in Propusher."
