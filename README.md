# OpenAI-API Playground 📕

## Achievements
✅ Built a receipt analyzer which I plan on incorporating into a budgget tracker application.
```python
# Function to encode the image
def encode_image(image_path):
  with open(image_path, "rb") as image_file:
    return base64.b64encode(image_file.read()).decode('utf-8')

# Path to your image (jpg)
image_path = "images/receipt_1.png"

# Getting the base64 string
base64_image = encode_image(image_path)

response = client.chat.completions.create(
  model="gpt-4o-mini",
  messages=[
    {
      "role": "user",
      "content": [
        {
          "type": "text",
          "text": "What are the items and price associated with them? Output each data point as a list [Item, Count, Total Price]",
        },
        {
          "type": "image_url",
          "image_url": {
            "url":  f"data:image/jpeg;base64,{base64_image}"
          },
        },
      ],
    }
  ],
)

print(response.choices[0].message.content)
```
### Output
```
Here are the items and their associated prices from the receipt:

1. [Burger, 1, $10.00]
2. [Salad, 1, $8.00]
3. [Soft Drink, 2, $10.00]
4. [Pie, 1, $10.00]
5. [Tax, 1, $4.50]
```
✅ Built a city activity recommender.  

```python
def city_activity(city):
  zero_shot_prompt = """Output a list of four reccomendations, line by line.
                      """

  # All GPT completions are generated by the chat model. The chat model works best
  # when it's given a baseline instruction about how GPT should behave.
  system_message = {"role" : "system", "content" : "Give a list of four activites to do in the given city, line by line."}

  client = openai.OpenAI()
  response = client.chat.completions.create(
      model="gpt-3.5-turbo",
      messages= [
          system_message,
          {"role" : "user", "content" : zero_shot_prompt + city + " - "} # simulate a user prompt
      ],
      temperature=0.7,
      max_tokens=256,
      top_p=1,
      frequency_penalty=0,
      presence_penalty=0,
      stop=["---"]
  )
  
  time.sleep(1)

  # the response from OpenAI's API is a JSON object that contains the completion to your prompt plus some other information.  
  # Here's how to access just the text of the completion.
  list = response.choices[0].message.content.strip()
  return list


city = """
Denver
"""

answer = city_activity(city)
print(answer)
```
### Output
```
1. Visit the Red Rocks Amphitheatre for a concert or hike among the stunning rock formations.
2. Explore the Denver Art Museum to see a diverse collection of art from around the world.
3. Take a stroll through the historic Larimer Square for shopping, dining, and vibrant nightlife.
4. Head to Denver Botanic Gardens to admire beautiful plant displays and peaceful gardens.
```
✅ Created a meal prep plan generator that takes in allergies and fitness goals.  
``` python
def meal_plan(allergies, fitness_goal):
  # TODO - write this function
  zero_shot_prompt = """Given the dietary restrictions and fitness goals, output a list of what to eat during the week.
                      """

  # All GPT completions are generated by the chat model now. The chat model works best
  # when it's given a baseline instruction about how GPT should behave. We'll use this one.
  system_message = {"role" : "system", "content" : "Output a list of what to eat in the week based on the dietary restrictions and fitness goals."}

  client = openai.OpenAI()
  response = client.chat.completions.create(
      model="gpt-3.5-turbo",
      messages= [
          system_message,
          {"role" : "user", "content" : zero_shot_prompt + allergies + fitness_goal + " - "} # simulate a user prompt
      ],
      temperature=0.7,
      max_tokens=256,
      top_p=1,
      frequency_penalty=0,
      presence_penalty=0,
      stop=["---"]
  )
  
  time.sleep(1)

  # the response from OpenAI's API is a JSON object that contains
  # the completion to your prompt plus some other information.  Here's how to access
  # just the text of the completion.
  summary = response.choices[0].message.content.strip()
  return summary

allergies = """
Peanuts, strawberries, gluten.
"""
fitness_goals = """
I would like to gain 20 pounds of muscle in the next year. 
"""


answer = meal_plan(allergies, fitness_goals)
print(answer)
```

### Output
```
Based on your dietary restrictions of avoiding peanuts, strawberries, and gluten, here is a sample meal plan for the week to help you gain muscle:

Monday:
- Breakfast: Scrambled eggs with spinach and avocado
- Snack: Greek yogurt with honey and almonds
- Lunch: Grilled chicken with quinoa and roasted vegetables
- Snack: Carrot sticks with hummus
- Dinner: Baked salmon with sweet potato and asparagus

Tuesday:
- Breakfast: Oatmeal with almond butter and banana
- Snack: Apple slices with almond butter
- Lunch: Turkey and avocado wrap with lettuce
- Snack: Rice cakes with tuna salad
- Dinner: Beef stir-fry with broccoli and brown rice

Wednesday:
- Breakfast: Smoothie with spinach, banana, protein powder, and almond milk
- Snack: Cottage cheese with pineapple
- Lunch: Lentil soup with a side salad
- Snack: Celery sticks with peanut butter (if tolerated)
- Dinner: Grilled shrimp with quinoa and steamed green beans

Thursday:
- Breakfast: Greek yogurt with granola and mixed berries (excluding strawberries)
- Snack: Rice cakes with hummus
- Lunch: Turkey chili with a side of cornbread
```




## Purpose:
#### This repsoitory is used as a playground to experiment with OpenAI's gpt-3.5-turbo API and learn about its abilities. 

## Next Goal:
#### 🔜 Incorporate OpenAI Vision API in the notebook to experiment with AI image analysis. 
