# AI Dog Matchmaker!
## 🐶 Summary: AI Dog Matchmaker!
### 🚀 Overview
This project is an intelligent chatbot that helps users find their ideal dog breed based on lifestyle, preferences, and household traits. It combines natural language processing, fuzzy string matching, and real-time image retrieval from GitHub for a personalized experience.

### 🧠 How It Works
💬 Conversational Interview:
The chatbot asks friendly questions about your activity level, family, shedding tolerance, and more.

### 🗣️ Natural Language Understanding:
Answers are interpreted flexibly using keyword and fuzzy matching, so you can reply in your own words. Currently simple responses are understood.

### 📊 Preference Scoring:
Each response is converted to a numeric score (1-5) for traits like energy, trainability, and compatibility.

### 🔎 Breed Data Matching:
Your preferences are compared to a database of dog breeds, calculating a compatibility score for each.

### 🖼️ Fuzzy Image Lookup:
The chatbot uses robust string matching to find the right breed folder on GitHub and fetches a random image.

### 🏅 Personalized Recommendations:
Top breeds are presented with images and clear explanations of why they fit your profile.

### ✨ Key Features
Handles conversational, natural language input
Robust breed and image lookup
Uses public GitHub API for breed images
Clear, actionable recommendations with explanations

```
import pandas as pd
dog_breeds = pd.read_csv('data/breed_traits.csv')
trait_descriptions = pd.read_csv('data/trait_description.csv')

import os
import random
import re
import requests
import matplotlib.pyplot as plt
import ipywidgets as widgets
import time
from typing import Dict, List, Tuple, Any
from rapidfuzz import process, fuzz
from urllib.parse import quote
from io import BytesIO
from PIL import Image
from torchvision import transforms
from IPython.display import display

class DogChatbot:
    def __init__(self):
        self.user_preferences = {}
        self.breed_data = dog_breeds
        self.github_repo_url = "https://api.github.com/repos/maartenvandenbroeck/Dog-Breeds-Dataset/contents/"
        self.setup_questions()

    def setup_questions(self):
        """Setup intelligent questions with natural language processing"""
        self.questions = [
            {
                "id": "children",
                "question": "Do you have young children in your household?",
                "trait": "Good With Young Children",
                "natural_language_keywords": {
                    "yes": ["yes", "have kids", "children", "toddlers", "babies", "family with kids", "I have kids"],
                    "no": ["no", "no kids", "no children", "adults only", "empty nest"]
                }
            },
            {
                "id": "allergies_drooling",
                "question": "Are you or anyone in your family sensitive to dog drooling or saliva?",
                "trait": "Drooling Level",
                "reverse_scale": True,  # Lower drooling is better for allergies
                "natural_language_keywords": {
                    "yes": ["allergic", "sensitive", "hate drooling", "slobber bothers me", "clean freak"],
                    "no": ["don't mind", "no problem", "not allergic", "love slobbery kisses"]
                }
            },
            {
                "id": "energy_lifestyle",
                "question": "What's your lifestyle like? Are you very active or more of a homebody?",
                "trait": "Energy Level",
                "natural_language_keywords": {
                    "high": ["very active", "love hiking", "runner", "outdoorsy", "energetic", "athletic", "I am very active"],
                    "medium": ["moderately active", "regular walks", "weekend activities", "balanced"],
                    "low": ["homebody", "prefer indoors", "low key", "couch potato", "quiet lifestyle"]
                }
            },
            {
                "id": "other_pets",
                "question": "Do you have other dogs or pets at home?",
                "trait": "Good With Other Dogs",
                "natural_language_keywords": {
                    "yes": ["have dogs", "other pets", "multi-pet household", "cats and dogs"],
                    "no": ["first pet", "no other animals", "only dog", "single pet"]
                }
            },
            {
                "id": "shedding_tolerance",
                "question": "How do you feel about dog hair around your house?",
                "trait": "Shedding Level",
                "reverse_scale": True,  # Lower shedding preferred if they mind hair
                "natural_language_keywords": {
                    "mind": ["hate hair", "allergic to fur", "clean house", "don't like mess"],
                    "don't mind": ["don't care", "part of having dogs", "love fluffy dogs", "fine with hair"]
                }
            },
            {
                "id": "training_commitment",
                "question": "How important is it that your dog learns commands quickly and easily?",
                "trait": "Trainability Level",
                "natural_language_keywords": {
                    "very important": ["very important", "must be trainable", "need obedient dog", "competitive training"],
                    "somewhat": ["somewhat important", "basic training", "house training mainly"],
                    "not important": ["don't care", "love them as they are", "not worried about training"]
                }
            },
            {
                "id": "noise_tolerance",
                "question": "How do you feel about a dog that barks frequently?",
                "trait": "Barking Level",
                "reverse_scale": True,  # Lower barking preferred if they mind noise
                "natural_language_keywords": {
                    "mind": ["hate barking", "need quiet", "apartment living", "noise sensitive"],
                    "don't mind": ["don't mind", "like vocal dogs", "want guard dog", "rural area"]
                }
            }
        ]

    def display_welcome_message(self):
        """Welcome message"""
        print("🐕 Hello! I'm your AI Dog Matchmaker! 🐕")
        print("=" * 60)
        print("I'll have a friendly conversation with you to understand")
        print("your lifestyle and preferences, then recommend the perfect")
        print("dog breeds for you!")
        print("=" * 60)
        print("\n💬 Feel free to answer naturally - I try to understand simple conversational responses!")
        print("You can say things like 'I have two young kids' or 'I'm not very active'")
        print()

    def interpret_natural_language(self, response: str, question_data: Dict) -> Any:
        """Interpret natural language responses using keyword matching and context"""
        response = response.lower().strip()

        # Check for explicit yes/no first
        if any(word in response for word in ["yes", "yeah", "yep", "definitely", "absolutely"]):
            return "yes"
        elif any(word in response for word in ["no", "nope", "not really", "don't", "never"]):
            return "no"

        # Check question-specific keywords
        if "natural_language_keywords" in question_data:
            keywords = question_data["natural_language_keywords"]

            # Prepare normalized folder list
            normalized_map = { self.normalize_name(f): f for f in keywords }

            match = self.normalize_name(response)

            # ✅ Check for exact match first
            if match in normalized_map:
                return normalized_map[match]

            # Otherwise use fuzzy matching
            match_norm, score, _ = process.extractOne(
                match,
                normalized_map.keys(),
                scorer=fuzz.WRatio
            )
            if score >= 65:
                response = normalized_map[match_norm]

            # Score each category
            scores = {}
            for category, keyword_list in keywords.items():
                score = 0
                for keyword in keyword_list:
                    if keyword in response:
                        score += 1
                scores[category] = score

            # Return the category with highest score
            if scores:
                return(max(scores.items(), key=lambda x: x[1])[0])

        # Fallback: ask for clarification
        return "unclear"

    def convert_response_to_preference(self, response: str, question_data: Dict) -> int:
        """Convert interpreted response to numeric preference (1-5 scale)"""
        trait = question_data["trait"]
        reverse_scale = question_data.get("reverse_scale", False)

        # Handle energy level specially (3-point scale to 5-point scale)
        if trait == "Energy Level":
            if response in ["high", "very active"]:
                return 5
            elif response in ["medium", "moderately active"]:
                return 3
            elif response in ["low", "homebody"]:
                return 1
            elif response == "yes":
                return 4  # Assume yes means somewhat active
            elif response == "no":
                return 2  # Assume no means less active

        # Standard yes/no conversion
        if response == "yes":
            return 1 if reverse_scale else 5
        elif response == "no":
            return 5 if reverse_scale else 1

        # Handle mind/don't mind responses
        if response == "mind":
            return 1  # Want low levels of this trait
        elif response == "don't mind":
            return 5  # OK with high levels

        # Handle importance levels
        if response == "very important":
            return 5
        elif response == "somewhat":
            return 3
        elif response == "not important":
            return 1

        # Default to neutral
        return 3

    def ask_question(self, question_data: Dict) -> int:
        """Ask a question with natural language processing"""
        question_text = question_data["question"]
        trait = question_data["trait"]

        print(f"\n🤖 {question_text}")

        # Get initial response
        while True:
            response = input("💬 Your answer: ").strip()
            if not response:
                print("😊 Please share your thoughts - I'm here to help!")
                continue

            # Interpret the response
            interpreted = self.interpret_natural_language(response, question_data)

            if interpreted == "unclear":
                print("🤔 I'm not sure I understood. Could you clarify?")
                print("💡 Try answering with 'yes/no' or describing your situation.")
                continue

            break

        # Convert to preference score
        preference_score = self.convert_response_to_preference(interpreted, question_data)

        return preference_score

    def conduct_interview(self):
        """Conduct the full interview with natural language processing"""
        self.display_welcome_message()

        # Get user's name for personalization
        name = input("🏷️  What should I call you? ").strip()
        if name:
            print(f"\nNice to meet you, {name}! 😊")
        else:
            name = "friend"

        print(f"\n{name}, I'm going to ask you some questions to understand what kind of dog would be perfect for you.")

        # Ask all questions
        for question_data in self.questions:
            score = self.ask_question(question_data)
            self.user_preferences[question_data["trait"]] = score

        print(f"\n✨ Perfect, {name}! I have everything I need to find your ideal matches!")
        return name

    def calculate_breed_match_score(self, breed_row: pd.Series) -> float:
        """Calculate how well a breed matches user preferences"""
        total_score = 0
        max_possible_score = 0

        for trait, user_pref in self.user_preferences.items():
            if trait in breed_row.index:
                breed_value = breed_row[trait]

                # Calculate compatibility (closer values = better match)
                difference = abs(breed_value - user_pref)
                compatibility = max(0, 5 - difference)  # 5 = perfect match, 0 = complete mismatch

                # Weight important traits more heavily
                weight = 1.0
                if trait in ["Good With Young Children", "Energy Level", "Drooling Level"]:
                    weight = 1.5

                total_score += compatibility * weight
                max_possible_score += 5 * weight

        return (total_score / max_possible_score) * 100 if max_possible_score > 0 else 0

    def get_top_recommendations(self, top_n: int = 3) -> List[Tuple[str, float, Dict]]:
        """Get top breed recommendations with detailed scoring"""
        recommendations = []

        for _, breed_row in self.breed_data.iterrows():
            match_score = self.calculate_breed_match_score(breed_row)
            breed_info = breed_row.to_dict()
            recommendations.append((breed_row['Breed'], match_score, breed_info))

        # Sort by match score (descending)
        recommendations.sort(key=lambda x: x[1], reverse=True)
        return recommendations[:top_n]

    def explain_recommendation(self, breed_name: str, breed_info: Dict) -> List[str]:
        """Generate explanations for why this breed was recommended"""
        explanations = []

        for trait, user_pref in self.user_preferences.items():
            if trait in breed_info:
                breed_value = breed_info[trait]
                difference = abs(breed_value - user_pref)

                if difference <= 1:  # Good match
                    if trait == "Good With Young Children" and user_pref >= 4:
                        explanations.append(f"Excellent with children (rating: {breed_value}/5)")
                    elif trait == "Energy Level":
                        energy_desc = ["very calm", "calm", "moderate", "active", "very active"][breed_value-1]
                        explanations.append(f"Energy level is {energy_desc} (rating: {breed_value}/5) - matches your lifestyle")
                    elif trait == "Drooling Level" and user_pref <= 2:
                        if breed_value <= 2:
                            explanations.append(f"Low drooling (rating: {breed_value}/5) - great for allergy concerns")
                    elif trait == "Trainability Level" and user_pref >= 4:
                        if breed_value >= 4:
                            explanations.append(f"Highly trainable (rating: {breed_value}/5)")
                    elif trait == "Shedding Level" and user_pref <= 2:
                        if breed_value <= 2:
                            explanations.append(f"Minimal shedding (rating: {breed_value}/5) - less hair around the house")
                    elif trait == "Barking Level" and user_pref <= 2:
                        if breed_value <= 2:
                            explanations.append(f"Quiet breed (rating: {breed_value}/5) - minimal barking")
                    elif trait == "Good With Other Dogs" and user_pref >= 4:
                        explanations.append(f"Very social with other dogs (rating: {breed_value}/5) - great for multi-dog homes")

        if not explanations:
            explanations.append("Good overall compatibility with your preferences")

        return explanations[:3]  # Limit to top 3 explanations

    def get_github_content(self, url):
        """Get github folder contents using the API."""
        try:
            response = requests.get(url)
            response.raise_for_status()
            return response.json()           
        except Exception as e:
            time.sleep(20)
            self.get_github_content(url)   
            return ""

    def get_github_folders(self):
        """Fetch top-level folder names from a public GitHub repo using the API."""
        self.github_data = self.get_github_content(self.github_repo_url)
        folders = [item["name"] for item in self.github_data if item["type"] == "dir"]
        return list(folders)

    def normalize_name(self, text: str) -> str:
        """
        Normalization:
        - Lowercase
        - Remove punctuation, underscores, hyphens
        - Remove trailing 'dog'
        - Remove plural 's' at the end
        """
        text = text.lower().strip()
        text = re.sub(r'[_\-]', ' ', text)          # underscores, hyphens -> space
        text = re.sub(r'[^a-z0-9\s]', '', text)     # remove non-alphanumeric
        text = re.sub(r'\s+', ' ', text).strip()    # normalize spaces

        # remove trailing 'dog'
        if text.endswith(" dog"):
            text = text[:-4].strip()

        # remove trailing plural 's' (e.g., dogues -> dogue)
        if text.endswith("s"):
            text = text[:-1]

        return text.strip()

    def find_closest_folder(self, breed_name):
        """Get the clostest matching folder name."""
        if not self.folder_names:
            return ""
        if breed_name[-1] == "s":
            breed_name = breed_name[:-1]
        words = breed_name.split()
        if words[-1] != "dog":
            breed_name += " dog"

        if breed_name.lower() in self.folder_names:
            return breed_name.lower()

        # Prepare normalized folder list
        normalized_map = { self.normalize_name(f): f for f in self.folder_names }

        breed_norm = self.normalize_name(breed_name)

        # Check for exact match first
        if breed_norm in normalized_map:
            return normalized_map[breed_norm]

        # Otherwise use fuzzy matching
        match_norm, score, _ = process.extractOne(
            breed_norm,
            normalized_map.keys(),
            scorer=fuzz.WRatio
        )

        if score >= 65:
            return normalized_map[match_norm]

        return ""

    def get_random_image_url(self, folder_path):
        """Get a random image url from the repo."""
        encoded_folder = quote(folder_path)
        url = f"{self.github_repo_url}{encoded_folder}"
        data = self.get_github_content(url)
        if not data:
            return ""
        images = [
            item["download_url"]
            for item in data
            if item["type"] == "file" and item["name"].lower().endswith((".jpg", ".jpeg"))
        ]
        return random.choice(images)

    def show_breed_image(self, breed_name, caption, footer):
        """Display image with caption"""
        best_folder = self.find_closest_folder(breed_name)
        if not best_folder:
            return ""

        image_url = self.get_random_image_url(best_folder)

        if not image_url:
            return ""

        try:
            response = requests.get(image_url)
            img = Image.open(BytesIO(response.content)).convert("RGB")

            transform = transforms.ToTensor()
            img_tensor = transform(img)

            # Display image with emoji caption
            plt.figure(figsize=(8, 8))
            plt.imshow(img_tensor.permute(1, 2, 0))  # CHW → HWC
            plt.axis("off")

            plt.title(caption, fontsize=16, pad=20, fontweight='bold', fontname='DejaVu Sans')
            
            plt.text(0.5, -0.08, footer,
                ha='center', va='top',
                fontsize=16,
                fontweight='bold', fontname='DejaVu Sans',
                transform=plt.gca().transAxes)
            plt.show()

        except Exception as e:
            return ""

    def display_results(self, recommendations: List[Tuple[str, float, Dict]], user_name: str):
        """Display results with detailed explanations"""
        print("\nLoading results. Please wait a moment....\n")
        print(f"\n🎯 {user_name.upper()}'S TOP DOG BREED MATCHES!")
        print("=" * 60)

        self.folder_names = self.get_github_folders()
        captions = ["Will you be my new best friend? ❤️", "It's me and you forever! ❤️", "Take me home! ❤️"]
        for i, (breed_name, match_score, breed_info) in enumerate(recommendations, 1):
            footer = f"#{i} - {breed_name}"
            self.show_breed_image(breed_name, captions[i - 1], footer)
            print(f"📊 Compatibility Score: {match_score:.1f}%")
            print("─" * 40)

            # Show key traits
            key_traits = ["Energy Level", "Good With Young Children", "Drooling Level", "Trainability Level", "Good With Other Dogs", "Barking Level", "Shedding Level"]
            for trait in key_traits:
                if trait in breed_info:
                    value = breed_info[trait]
                    stars = "★" * value + "☆" * (5 - value)
                    print(f"   {trait:<25} {stars} ({value}/5)")

            # Show explanations
            explanations = self.explain_recommendation(breed_name, breed_info)
            print("\n💡 Why this breed fits you:")
            for explanation in explanations:
                print(f"   • {explanation}")
            print("\n" + "="*50)

    def run_chatbot(self):
        """Main execution method"""
        try:
            # Conduct interview
            user_name = self.conduct_interview()

            # Get recommendations
            print("\n🔍 Analyzing your responses and finding perfect matches...")
            recommendations = self.get_top_recommendations(3)

            # Display results
            self.display_results(recommendations, user_name)

            print(f"\n🎉 Thanks for using the Dog Breed Chatbot, {user_name}! 🐾")
            print("I hope you find your perfect furry companion!")

        except KeyboardInterrupt:
            print("\n\n👋 Goodbye! Come back anytime to find your perfect dog!")
        except Exception as e:
            print(f"\n❌ Oops! Something went wrong: {e}")


def main():
    """Main function to run the chatbot"""
    chatbot = DogChatbot()
    chatbot.run_chatbot()

if __name__ == "__main__":
    main()
```

### Output:
