# 🥗 SNAP2MEAL — From Ingredients to Ideas

**SNAP2MEAL** is a multimodal Generative AI application that turns a picture of available ingredients into personalized recipe suggestions.

Instead of searching through recipes manually, users can simply upload a photo of their pantry or ingredients. The application uses a vision-capable Generative AI model to identify visible ingredients and generate recipes according to the user's **cuisine preference, dietary preference, meal type, and number of servings**.

---

## ✨ Features

* 📸 **Pantry Image Analysis**
  Upload an image of your available ingredients.

* 👁️ **Multimodal AI**
  Uses a vision-language model to understand both the uploaded image and text instructions.

* ✏️ **Add / Correct Ingredients**
  Users can manually add missing ingredients or correct the AI's interpretation.

* 🌍 **Cuisine Customization**
  Choose from cuisines such as:

  * Indian
  * Italian
  * Mexican
  * Asian / Fusion
  * Mediterranean
  * American

* 🥗 **Dietary Preferences**

  * Vegetarian
  * Vegan
  * Gluten-Free
  * High-Protein
  * Keto
  * Non-Vegetarian

* 🍽️ **Meal Type Selection**

  * Breakfast
  * Main Course
  * Quick Snack / Side
  * Soup / Salad

* 👥 **Serving Adjustment**
  Generate recipes for 1–6 servings.

* 📊 **Recipe Overview**
  Provides estimated calories, protein, preparation time, cooking time, and ingredients.

* 🛒 **Shopping Checklist**
  Suggests optional ingredients that could improve the recipes.

* 📥 **Recipe Download**
  Generated recipes can be downloaded as a `.txt` file.

---

## 🔄 How It Works

```text
User uploads pantry image
          ↓
       Gradio UI
          ↓
     NumPy Image
          ↓
     PIL / Pillow
          ↓
 Resize + JPEG Encoding
          ↓
      Base64 Encoding
          ↓
 Image + User Preferences
          ↓
    OpenRouter API
          ↓
 Vision-Capable LLM
          ↓
 Ingredient Understanding
          ↓
 Personalized Recipe Generation
          ↓
       Gradio Output
```

---

## 🧠 Generative AI Approach

SNAP2MEAL demonstrates several important Generative AI concepts:

### Multimodal AI

The model receives both:

* 🖼️ Image input
* 📝 Text prompt

This allows the model to understand the ingredients in the image while also following the user's instructions.

### Prompt Engineering

A structured prompt guides the model through multiple stages:

1. Identify visible ingredients
2. Incorporate manually added ingredients
3. Identify required staples
4. Create a shopping checklist
5. Generate three recipes
6. Apply dietary and cuisine constraints
7. Scale quantities according to servings

### Dynamic Prompting

User selections are dynamically inserted into the prompt.

For example:

```text
Cuisine: Indian
Dietary Preference: Vegetarian
Meal Type: Main Course
Servings: 2
```

These values influence the generated recipes.

### Constraint-Based Generation

The prompt contains rules such as:

```text
Do NOT invent main ingredients not visible in the image
or explicitly added by the user.
```

This helps keep the generated recipes aligned with the available ingredients.

### Human-in-the-Loop

The user can manually add or correct ingredients after the image is analyzed.

This creates a feedback step between AI interpretation and recipe generation.

---

## 🛠️ Technology Stack

| Technology            | Purpose                                   |
| --------------------- | ----------------------------------------- |
| **Python**            | Core application logic                    |
| **Gradio**            | Interactive web interface                 |
| **Pillow (PIL)**      | Image processing and resizing             |
| **OpenAI Python SDK** | API communication                         |
| **OpenRouter API**    | Access to Generative AI models            |
| **Multimodal LLM**    | Image understanding and recipe generation |
| **Google Colab**      | Development environment                   |

---

## 🖼️ Image Processing

The uploaded image goes through the following preprocessing pipeline:

```text
Uploaded Image
      ↓
NumPy Array
      ↓
PIL Image
      ↓
Resize to max 512 × 512
      ↓
JPEG Conversion
      ↓
Base64 Encoding
      ↓
API Request
```

Pillow is responsible for **image preprocessing**, while the actual ingredient understanding is performed by the vision-capable Generative AI model.

---

## 🔌 API Integration

The project uses the OpenAI Python SDK with OpenRouter's OpenAI-compatible API.

```python
client = OpenAI(
    base_url="https://openrouter.ai/api/v1",
    api_key=api_key
)
```

The application currently uses:

```text
openrouter/free
```

This allows the project to experiment with available free models capable of handling the required multimodal task.

---

## 🔐 API Key Security

The API key is **not hardcoded** into the source code.

It is retrieved from Google Colab Secrets:

```python
api_key = userdata.get("OPENROUTER_API_KEY")
```

Add your API key to Colab Secrets before running the application.

---

## 🚀 Getting Started

### 1. Clone the repository

```bash
git clone https://github.com/your-username/SNAP2MEAL.git
cd SNAP2MEAL
```

### 2. Install dependencies

```bash
pip install gradio openai pillow
```

### 3. Configure your API key

Create an OpenRouter API key and store it securely as:

```text
OPENROUTER_API_KEY
```

If using Google Colab, add it through **Secrets** and enable the notebook access permission.

### 4. Run the application

Run the Python/Colab notebook containing the SNAP2MEAL application.

The Gradio interface will launch and provide the recipe generator.

---

## 📋 Example Workflow

**Input:**

User uploads an image containing:

```text
Tomatoes
Onion
Potatoes
Capsicum
```

Then selects:

```text
Cuisine: Indian
Diet: Vegetarian
Meal Type: Main Course
Servings: 2
```

**Output:**

The AI generates three recipe suggestions using the identified ingredients, along with:

* Ingredients
* Cooking time
* Preparation time
* Estimated nutrition
* Instructions
* Shopping checklist

---

## 🏗️ Project Architecture

```text
                    ┌─────────────────────┐
                    │       User          │
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │    Gradio UI        │
                    │ Image + Preferences │
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │ Image Preprocessing │
                    │ NumPy + Pillow      │
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │ Prompt Construction │
                    │ Dynamic User Input  │
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │   OpenRouter API    │
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │  Vision-Language    │
                    │        LLM          │
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │ Personalized       │
                    │ Recipe Generation   │
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │ Gradio Results      │
                    │ + Download          │
                    └─────────────────────┘
```

---

## ❓ Is SNAP2MEAL RAG?

**No.**

The current version does not use:

* ❌ Vector database
* ❌ Embeddings
* ❌ Retriever
* ❌ External recipe knowledge base

The current architecture is:

```text
Image + Prompt
      ↓
Vision LLM
      ↓
Generated Recipes
```

RAG can be added in a future version using a verified recipe and nutrition database.

---

## 🤖 Is SNAP2MEAL an AI Agent?

The current version is **not an autonomous AI agent**.

It follows a predefined workflow:

```text
Input → Process → Generate → Output
```

There is no autonomous tool selection or multi-step tool execution loop.

An agent-based version could later integrate:

* Recipe databases
* Nutrition APIs
* Grocery APIs
* Expiry tracking
* Shopping services

---

## ⚠️ Limitations

* Ingredient recognition may not always be perfect.
* Nutrition values are AI-generated estimates and should not be treated as medically verified.
* Recipe quality depends on the underlying model.
* The application requires an internet connection and API access.
* The free-model router may provide different underlying models at different times.
* The current version does not use a verified recipe or nutrition database.

---

## 🔮 Future Scope

### 📚 RAG-Based Recipe Knowledge

Integrate a verified recipe database using:

```text
Recipe Database
      ↓
Embeddings
      ↓
Vector Database
      ↓
Retriever
      ↓
LLM
```

This could improve factual consistency.

### 🥦 Nutrition Integration

Connect reliable nutrition APIs to provide more accurate nutritional information.

### ⏳ Expiry Tracking

Allow users to enter purchase dates or expiry dates and prioritize ingredients that need to be consumed first.

### 🛒 Smart Grocery Integration

Automatically convert missing ingredients into a shopping list and potentially integrate with grocery platforms.

### 🧑‍🍳 Conversational Chef

Add a chatbot where users can ask follow-up questions such as:

> "Make this recipe less spicy."

> "What can I substitute for onion?"

> "Make it high-protein."

---

## 👩‍💻 Team

Built as a team project during **Samsung Innovation Campus Phase 2 AI Training**.

The project helped us apply concepts including:

* Generative AI
* Multimodal AI
* LLMs
* Prompt Engineering
* Image Processing
* API Integration
* Human-in-the-Loop AI

---

## 🎯 Project Objective

The goal of SNAP2MEAL is simple:

> **Turn what you already have into something you can cook.**

Instead of asking *"What recipe should I search for?"*, SNAP2MEAL starts with the ingredients you already have and generates ideas around them.

---

## 📄 License

This project is intended for educational and demonstration purposes.
