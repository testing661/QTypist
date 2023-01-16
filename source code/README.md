We give source code (./source code/). Please note that our LLM uses OpenAI API. Please replace it with your OpenAI key. Thank you very much for your support for our work!

The use of OpenAI API has been described in detail in Readme. The generation method of training data is also given in Readme.


As shown in the following table, it is all our patterns.

<table>
    <tr>
        <td><b>Id</b></td>
        <td><b>Sample of linguistic patterns/rules</b></td>
        <td><b>Examples of linguistic patterns/rules</b></td>
    </tr>
    <tr>
        <td colspan = "3"><b>Patterns related to input widget: IWPn</b> </td>
    </tr>
    <tr>
        <td>1</td>
        <td>Please input < widget[n] >, the < widget[n] > is</td>
        <td>Please input game name, the game name is</td>
    </tr>
    <tr>
        <td>2</td>
        <td>Please input < widget[det+n] >, < widget[det+n] > is</td>
        <td>Please input your nickname, your nickname is</td>
    </tr>
    <tr>
        <td>3</td>
        <td>Please < widget[v+n] >, the < widget[n] > is</td>
        <td>Please search the food, the food is</td>
    </tr>
    <tr>
        <td>4</td>
        <td>Please < widget[v] > </td>
        <td>Please search </td>
    </tr>
    <tr>
        <td>5</td>
        <td>< widget[n] > + $[MASK]$ + < widget[n] ></td>
        <td>Your weight is [MASK] kg</td>
    </tr>
    <tr>
        <td>6</td>
        <td>< widget[n] > + $[MASK]$ </td>
        <td>Your age is [MASK]</td>
    </tr>
    <tr>
        <td>7</td>
        <td>< widget[prep] > + $[MASK]$</td>
        <td>From [MASK]</td>
    </tr>
    <tr>
        <td colspan = "3"><b>Patterns related to local context: LCPn</b> </td>
    </tr>
    <tr>
        <td>8</td>
        <td>< widget[prep] > + $[MASK]$, < widget[prep] > + $[MASK]$</td>
        <td>From [MASK], to [MASK]</td>
    </tr>
    <tr>
        <td>9</td>
        <td>This input is about < local[n] ></td>
        <td>This input is about the NBA team.</td>
    </tr>
    <tr>
        <td>10</td>
        <td>This input is about < local[n] >, we need to < local[v+n] ></td>
        <td>This input is about one-way flight, we need to search the flight information.</td>
    </tr>
    <tr>
        <td>11</td>
        <td>This input is about < local[n] >, please < local[v] > </td>
        <td>This input is about your health, please input.</td>
    </tr>
    <tr>
        <td>12</td>
        <td>This input is about < local[n] >, we need to input < local[n] ></td>
        <td>This input is about one-way train, we need to input the seat map.</td>
    </tr>
    <tr>
        <td>13</td>
        <td>This input is about < local[n] >, we need to known it < local[prep] ></td>
        <td>This input is about your trip, we need to know it from.</td>
    </tr>
    <tr>
        <td colspan = "3"><b>Patterns related to global context: GCPn</b> </td>
    </tr>
    <tr>
        <td>14</td>
        <td>This is < app\ name > app, in its < activity\ name > page, the input category is < input\ category >.</td>
        <td>This is a NBA sport app, in its search the NBA team page, the input category is query category.</td>
    </tr>
    <tr>
        <td colspan = "3"><b>Prompt generation rules</b> </td>
    </tr>
    <tr>
        <td>1</td>
        <td>< GCPtn > + < LCPtn > + < IWPtn ></td>
        <td>This is a my movie app, in its search movie page, the input category is query category.  This input is about your favorite move in this year. Please search the movie, the movie is </td>
    </tr>
    <tr>
        <td>2</td>
        <td>< GCPtn > + [< LCPtn > + < IWPtn >]{n}</td>
        <td>This is a money wallet app, in its personal income page, the input category is numeric category. This input is about your monthly income. Income is [MASK] dollar. This input is about your expenses. Expenses is [MASK] dollar.</td>
    </tr>
</table>


You can get the code and tuning data through our code.

Fine tune your gpt-3 as follows, and the effect is the same.

1.We recommend using our OpenAI command-line interface (CLI). To install this, run
`pip install --upgrade openai`

2.(The following instructions work for version 0.9.4 and up. Additionally, the OpenAI CLI requires python 3.)

Set your OPENAI_API_KEY environment variable by adding the following line into your shell initialization script (e.g. .bashrc, zshrc, etc.) or running it in the command line before the fine-tuning command:

`export OPENAI_API_KEY="<OPENAI_API_KEY>"`

3.Prepare training data

Training data is how you teach GPT-3 what you'd like it to say.
Your data must be a JSONL document, where each line is a prompt-completion pair corresponding to a training example. You can use CLI data preparation tool to easily convert your data into this file format.

`{"prompt": "<prompt text>", "completion": "<ideal generated text>"}`

4. CLI data preparation tool
We developed a tool which validates, gives suggestions and reformats your data:

`openai tools fine_tunes.prepare_data -f <LOCAL_FILE>`

5. Create a fine-tuned model

`openai api fine_tunes.create -t <TRAIN_FILE_ID_OR_PATH> -m Curie`

6. After you've started a fine-tune job, it may take some time to complete. Your job may be queued behind other jobs on our system, and training our model can take minutes or hours depending on the model and dataset size. If the event stream is interrupted for any reason, you can resume it by running:

`openai api fine_tunes.follow -i <YOUR_FINE_TUNE_JOB_ID>`

7. Use a fine-tuned model

`openai api completions.create -m <FINE_TUNED_MODEL> -p <YOUR_PROMPT>`

`curl https://api.openai.com/v1/completions \
  -H "Authorization: Bearer $OPENAI_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"prompt": YOUR_PROMPT, "model": FINE_TUNED_MODEL}'`
  
 `
import openai
openai.Completion.create(
    model=FINE_TUNED_MODEL,
    prompt=YOUR_PROMPT)`

Since the API of gpt-3 contains personal information, we will give our fine tuned API after the double-blind review.

The key words of our approach are shown in the table.
