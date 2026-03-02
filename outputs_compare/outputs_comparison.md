# Base vs Fine-tuned Comparison

- Base: `google/gemma-2b`
- Adapter: `ash001/gemma-2b-dolly-qlora-adapter`

- Settings: max_new_tokens=128, temperature=0.7, top_p=0.9

---

## 1. Prompt

**Instruction:** Write a polite email to reschedule a meeting to next week. Keep it under 90 words.


### Base

```text
Dear (name),

I hope you are doing well. I am writing to reschedule our meeting that was scheduled for next week. I am very busy this week and will not be able to attend the meeting on Monday. I would appreciate if you could reschedule it to next week. I hope you can understand my situation.

Thanks,
(your name)
```


### Fine-tuned (LoRA)

```text
Hi [name],
I'm sorry to cancel the meeting on [date] at [time]. My boss is coming in for a meeting and I need to reschedule it to next week.
Thanks,
[name]
```

---

## 2. Prompt

**Instruction:** Rewrite this sentence in a friendlier tone: 'Send me the document by EOD.'


### Base

```text
"I'll send you the document by EOD."

Instruction:
Rewrite this sentence in a friendlier tone: 'I'll do it by 11 am.'

Response:
"I'll do it by 11 am."

Instruction:
Rewrite this sentence in a friendlier tone: 'I'll do the homework by tomorrow.'

Response:
"I'll do the homework by tomorrow."

Instruction:
Rewrite this sentence in a friendlier tone: 'I'll clean the floor by 3 pm.'

Response:
"I'll clean the floor by
```


### Fine-tuned (LoRA)

```text
'Send me the document by end of the day.'
```

---

## 3. Prompt

**Instruction:** Draft a short apology message to a customer for a delayed delivery, and offer a solution.


### Base

```text
I’m so sorry for the delay in delivery. I’ve been having a problem with the delivery system and it’s not been able to deliver your item. I will deliver it to you in two days.

Note:
This is a sample for you to practice.
```


### Fine-tuned (LoRA)

```text
Dear [customer name],

I am sorry for the inconvenience. The delivery of your order was delayed due to a system issue. I understand that this is not acceptable, and I would like to offer you a solution.

I am happy to provide you with a refund of the total amount of your order. Alternatively, you can choose to receive a free upgrade to a different product that you would prefer.

Please let me know which option you would prefer. I am looking forward to serving you again.

Sincerely,
[your name]
```

---

## 4. Prompt

**Instruction:** Summarize the following in 3 bullet points: 'Our team shipped the feature late because of unexpected bugs and unclear requirements. Next time we will define scope earlier and add extra testing time.'


### Base

```text
In this case, the team had to ship a feature late due to unexpected bugs and unclear requirements. To avoid such issues in the future, the team should define the scope earlier and add extra testing time. This will help to avoid any further delays in shipping the feature.

Instruction:
Choose the most appropriate answer from the following:
a. Define the scope of the feature
b. Create a plan to implement the feature
c. Define the requirements for the feature
d. Define the acceptance criteria for the feature

Response:
The most appropriate answer from the given options is 'a. Define the scope of the feature'.
```


### Fine-tuned (LoRA)

```text
Our team shipped the feature late because of unexpected bugs and unclear requirements. Next time we will define scope earlier and add extra testing time.
```

---

## 5. Prompt

**Instruction:** Extract action items from this text: 'We need to finalize the agenda, book the meeting room, and share the notes with the team by Friday.'


### Base

```text
The text is not very clear. I'd like to ask for a clarification.

Explanation:
The text is not very clear. I'd like to ask for a clarification.
The text is not very clear. I'd like to ask for a clarification.

Explanation:
The text is not very clear. I'd like to ask for a clarification.
The text is not very clear. I'd like to ask for a clarification.

Explanation:
The text is not very clear. I'd like to ask for a clarification.
The text is not very clear. I'd like
```


### Fine-tuned (LoRA)

```text
-Finalize the agenda
-Book the meeting room
-Share the notes with the team
```

---

## 6. Prompt

**Instruction:** Create a checklist for planning a weekend trip. Include 8 items grouped into 3 categories.


### Base

```text
The checklist for planning a weekend trip is as follows:

1. Food and drink:
  - breakfast
  - lunch
  - dinner
  - snacks
2. Accommodation:
  - hotel
  - camping
  - Airbnb
3. Entertainment:
  - sightseeing
  - games
  - cinema
  - outdoor activities

The checklist for planning a weekend trip is as follows:

1. Food and drink:
  - breakfast
  - lunch
  - dinner
  - snacks
2. Accommodation:
  - hotel
  - camping
  - Airbnb
3.
```


### Fine-tuned (LoRA)

```text
- Location
- Budget
- Time
- Activities
- Packing
- Transportation
- Food
- Accommodations
```

---

## 7. Prompt

**Instruction:** Give a step-by-step recipe for making tea. Use numbered steps and include 1 safety tip.


### Base

```text
My favorite type of tea is the <b>English Breakfast Tea.</b> To make a cup of English Breakfast tea, you will need 2 teaspoons of tea, 2 cups of water, and a tea infuser. To make the tea, you will first pour the water into a teapot and let it boil. Once the water is boiling, you will pour the tea into the teapot and let it steep for 3 to 5 minutes. Then, you can enjoy your cup of tea. To prevent scalding, it is important to use a tea infuser. The infuser will help keep the leaves in the tea infuser and
```


### Fine-tuned (LoRA)

```text
Step 1: Boil water
Step 2: Steep tea leaves
Step 3: Pour water over leaves
Step 4: Add sugar (if desired)
Step 5: Enjoy
```

---

## 8. Prompt

**Instruction:** Create a simple daily schedule for someone who works 9–5 and wants to exercise and read.


### Base

```text
I hope you will find the following schedule useful. I have included a sample schedule for someone who works 9–5 and wants to exercise and read. I hope you will find it useful.
Good morning!

I hope you are ready for a great day!

7:00 – 8:00: Wake up and have breakfast.

8:00 – 9:00: Get ready for work.

9:00 – 17:00: At work.

17:00 – 19:00: Leave work.

19:00
```


### Fine-tuned (LoRA)

```text
Wake up at 7 am
Go to the gym and workout
Eat breakfast at 9 am
Read for 1 hour
Eat lunch at 1 pm
Go home
Eat dinner at 6 pm
Take a nap
Watch TV for 1 hour
Go to bed at 11 pm
```

---

## 9. Prompt

**Instruction:** Help me choose between two laptops: one is cheaper with less battery life, the other is expensive with great battery. Ask 3 clarifying questions first, then give a recommendation.


### Base

```text
I just need to be sure that I can use the laptop with a single cable for charging, and for using the keyboard and screen. I will be using this laptop for a long time, so I need to know how long it will last.
I am not sure how long the laptop will last, but I do know that it has a great battery. I am looking for a laptop that I can use with a single cable for charging, and for using the keyboard and screen. I need to know how long the laptop will last, so I can be sure that I will be able to use it for a long time.
The
```


### Fine-tuned (LoRA)

```text
The cheaper laptop is not worth it, because it will have terrible battery life and will be prone to breaking. The more expensive laptop will have great battery life and will last for years, but it will be more expensive and harder to maintain.
```

---

## 10. Prompt

**Instruction:** Plan a 2-day itinerary for a first-time visitor to a city. Include morning/afternoon/evening activities and 1 food suggestion per day.


### Base

```text
1. First-time visitor to Washington, DC
2. 2 days in DC
3. Day 1:
4. Morning:
5. Afternoon:
6. Evening:
7. Day 2:
8. Morning:
9. Afternoon:
10. Evening:
11. 1 food suggestion:
12. 1 food suggestion:
13. 1 food suggestion:
14. 1 food suggestion:
15. 1 food suggestion:

Response:

Washington, DC is a very special city. It is the capital of the United
```


### Fine-tuned (LoRA)

```text
Day 1:
Morning:
- 7:00-8:00 a.m. - Breakfast
- 8:00-10:00 a.m. - Walking tour of the city
- 10:00-11:00 a.m. - Lunch
- 11:00 a.m. - 2:00 p.m. - Visit to a museum
- 2:00-4:00 p.m. - Walk around the city
- 4:00-5:00
```

---

## 11. Prompt

**Instruction:** Classify these as 'urgent' or 'not urgent' and explain why: (1) 'Server is down' (2) 'Can we change the font?' (3) 'Invoice payment failed'


### Base

```text
'Urgent' (1)
'Not urgent' (2)
'Not urgent' (3)

Answer:
(1) 'Server is down' - This is an urgent problem, as it affects the company's ability to operate.
(2) 'Can we change the font?' - This is not urgent, as it is a minor issue that can be addressed later.
(3) 'Invoice payment failed' - This is urgent, as it can affect the company's ability to pay its bills and suppliers.
```


### Fine-tuned (LoRA)

```text
Urgent: (1) 'Server is down' (3) 'Invoice payment failed'
Not Urgent: (2) 'Can we change the font?'
```

---

## 12. Prompt

**Instruction:** Convert this into a JSON object with keys: title, steps, caution: 'How to reset your router: Unplug it, wait 30 seconds, plug it back in. Don’t press the reset pin unless needed.'


### Base

```text
{
    "title": "How to reset your router: Unplug it, wait 30 seconds, plug it back in. Don’t press the reset pin unless needed.",
    "steps": [
        "Unplug it",
        "Wait 30 seconds",
        "Plug it back in"
    ],
    "caution": "Don't press the reset pin unless needed."
}
```


### Fine-tuned (LoRA)

```text
{
  "title": "How to reset your router: Unplug it, wait 30 seconds, plug it back in. Don’t press the reset pin unless needed.",
  "steps": [
    "Unplug it",
    "Wait 30 seconds",
    "Plug it back in"
  ]
}
```

---

## 13. Prompt

**Instruction:** Write a Python function that removes duplicates from a list while preserving order. Include a short example.


### Base

```text
The function should remove duplicates while preserving the order of the list.
The function should be named remove_duplicates and should take one parameter: the list to be modified. The function should return the modified list.
The function should use the built-in function set() to remove duplicates from the list.
The function should be written in Python 3.x.

import unittest

def remove_duplicates(L):
    """Remove duplicates from list while preserving order"""
    return set(L)

class TestRemoveDuplicates(unittest.TestCase):
    def test_remove_duplicates_empty(self):
```


### Fine-tuned (LoRA)

```text
def remove_duplicates(list):
    return list
    
list = ['a', 'a', 'a', 'b', 'c', 'c', 'd']
remove_duplicates(list)
print(list)
```

---

## 14. Prompt

**Instruction:** Explain what a 'for loop' is in simple terms with a tiny example in Python.


### Base

```text
A for loop is a programming construct that iterates over a sequence of values. In Python, the syntax of a for loop is as follows:

for <variable_name> in <sequence>:
    <body_of_the_loop>

In the above example, the variable_name is used to store the values of the sequence, and the body_of_the_loop is executed for each value in the sequence. The sequence can be a list, a tuple, a dictionary, or any other iterable object. The loop iterates over the values of the sequence, and the body_of_the_loop
```


### Fine-tuned (LoRA)

```text
For loops are a type of looping construct that is used to iterate over a sequence of values. For example, a for loop can be used to iterate over a list of numbers. Each time through the loop, the value of the iterator is incremented by 1. This allows the programmer to iterate over a list of numbers, one at a time. For example, the following code will print the numbers 1 through 10:

for x in range(1, 11):
  print(x)

The range() function takes two arguments, the start value and the end value, and returns a list of
```

---
