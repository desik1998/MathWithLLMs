# MathWithLLMs

This project aims to prove that LLMs can learn math when trained on a step by step procedural way similar to how humans do it. For now, I've done multiplication operation to illustrate how LLMs can learn multiplication when done in a step by step procedure. For example, instead of teaching LLMs multiplication like ```23 * 34 = 782```, we teach it mutliplication similar to how human does using a digit wise multiplication, get values for each digit multiplication and further add the resulting numbers to get the final result.

**1. Illustration:**

**1.1 Human multiplication:**
<pre>
  23
x 34
_____
  92
 690
_____
 782
</pre>


**1.2 On a Programatical Note:**
  ```
  23 x 34
  = 23 x (4 + 30)

  - First let's multiply by 4.
    - 23 * 4
      - 3 * 4 = 12, carry = 12/10 = 1, remainder1 = 12%10 = 2
      - 2 * 4 = 8, carry = (8 + prevCarry)/10 = (8 + 1)/10 = 0, remainder2 = (8 + 1)%10 = 9
      - Overall Number 1 = [remainder2, remainder1] = 92

  - Let's now multiply by 40
    - 23 * 30 = 23 * 3 * 10
      - 23 * 3
       - 3 * 3 = 9, carry = 9/10 = 0, remainder = 9%10 = 9
       - 2 * 3 = 6, carry = (6+1)/10 = 0, remainder = 6%10 = 6
       - Overall Number 2 = [remainder2, remainder1] * 10 = 690

  - Addition = Overall Number 1 + Overall Number 2 = 92 + 690
    - unit's digits = 2 + 0 = 2
    - ten's digits = 9 + 9 = 18, carry = 1, remainder = 8
    - hundred's digits = carry + 6 = 7, carry = 0, remainder = 7
    - Overall Number = 782
   ```

Why aren't today's state of the art models able to do multiplication?
Instructions on the internet are .......

## 2. Results

The benchmarking was done on 200 test cases where each test case has 2 random numbers generated. These 200 test cases were generated keeping in mind the OpenAI Gpt3.5 4096 token limit in mind. A 5*5 digit multiplication can in general fit within 4096 limit but 6*6 cannot fit. But if 1 number is 6 digit, the other can be 4 digit or less and similarly if 1 number is 7 digit then the other can be <= 3 digit. For the 200 samples which were tested, excluding for 3 cases, the rest of the cases the multiplication is correct. **Which means this overall accuracy is 98.5%**. 

**What is the overall training and validation loss when training (finetuning) the model?**

The training loss and validation loss reach near 0 within 100 steps (0.1 epochs) using the gpt3.5-turbo Model. This means that the model is able to follow and understand the procedure. 

![Training and validation loss](https://github.com/desik1998/MathWithLLMs/blob/main/Math%20Training%20and%20Validation%20Loss.png)


### 3. Questions

3.1 Why is Finetuning done and why wouldn't few shot prompting work?

A. When initially [tested using incontext learning for Gpt4, the model was able to follow the procedure showed in the 1shot prompting but it was sometimes failing with the overall result due to OpenAI's addition technique](https://chat.openai.com/share/4633c517-edad-420d-8689-36f5c4393557). And given this is a procedural technique, doing a n-shot prompting would lead to even more tokens. Given these limitations, it was evident that finetuning would solve these issue. And also the model was able to quickly learn the procedure and the overall training and validation loss of model went close to 0 within 0.1 epochs

3.2 Why is OpenAI's API used and not an OpenSource LLM?

A. This project is just to prove that LLMs can do Math when taught in a procedural way. And given this is just for proof purposes, proving with the state of the art model would be easier

3.3 How is finetuning Dataset generated?

A. The test cases were generated keeping in mind the OpenAI Gpt3.5 4096 token limit in mind. A 5*5 digit multiplication can in general fit within 4096 limit but 6*6 cannot fit. But if 1 number is 6 digit, the other can be <= 4 digit and similarly if 1 number is 7 digit then the other can be <= 3 digit

3.4 What can be done better?

A. Find a way to simplify the data instruction size so that token size can be reduced. This simplification has to be done such that overall accuracy of the model is not reduced much

3.5 What are the next steps?

- Reach out to AI and open source community to make this proposal better or identify any flaws
- Do the same process of finetuning using Open Source LLMs
- **Figure out what's the smallest LLM that can do Math accurately when trained in a procedural manner (A 10 year kid can do Math)**. Check this for both normal models and distilled models as well

3.6 Why aren't today's state of the models able to reverse engineer the multiplication algo?

A. Not sure. But my intution is, it's even difficult for a human to do so when not taught in a procedural way. Consider a scenario where a person is not taught multiplication using procedure but just using ```a * b = c```. Just using a bunch of examples, figuring out that the algo is ```digit wise multiplication using carry and further adding up the resulting numbers``` is very difficult for a human. 

3.7 Will scaling the state of the art models further help it in reverse engineering the math algo's?

A. As [Greg Brockman has mentioned during his TED Talk](https://www.linkedin.com/posts/seeall_chatgpt-gpt-gpt4-activity-7054916094439866368-SjgR/), Gpt4 is coming up with an Internal Representation to add 2 40 digit numbers. Similarly, maybe Scaling the current 1T Parameter Models to further might help the model to figure out the multiplication algo when only trained on ```a * b = c``` based examples.
