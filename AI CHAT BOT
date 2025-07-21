import spacy
from sklearn.feature_extraction.text import TfidfVectorizer
from sklearn.metrics.pairwise import cosine_similarity
import numpy as np


nlp = spacy.load("en_core_web_sm")


knowledge_base = {
    "What is natural language processing?": "Natural Language Processing (NLP) is a field of artificial intelligence that focuses on the interaction between computers and humans through natural language.",
    "Explain machine learning": "Machine learning is a subset of artificial intelligence that involves training algorithms to make predictions or decisions without being explicitly programmed.",
    "What is SpaCy?": "SpaCy is an open-source NLP library in Python used for advanced natural language processing tasks such as tokenization, named entity recognition, and dependency parsing.",
    "Tell me about AI": "Artificial intelligence (AI) refers to the simulation of human intelligence in machines that are programmed to think and learn.",
    "How does TF-IDF work?": "TF-IDF stands for Term Frequency-Inverse Document Frequency. It is a numerical statistic that reflects how important a word is to a document relative to a collection of documents."
}


questions = list(knowledge_base.keys())
answers = list(knowledge_base.values())

def preprocess_text(text):
    doc = nlp(text.lower())
    return " ".join([token.lemma_ for token in doc if not token.is_stop and token.is_alpha])

preprocessed_questions = [preprocess_text(q) for q in questions]


tfidf_vectorizer = TfidfVectorizer()
question_vectors = tfidf_vectorizer.fit_transform(preprocessed_questions)

def get_response(user_query):
    """Generate a response based on user query."""
   
    preprocessed_query = preprocess_text(user_query)
    query_vector = tfidf_vectorizer.transform([preprocessed_query])

    
    similarities = cosine_similarity(query_vector, question_vectors)
    best_match_idx = np.argmax(similarities)

    
    if similarities[0][best_match_idx] > 0.2:
        return answers[best_match_idx]
    else:
        return "I'm sorry, I don't understand your question. Could you please rephrase?"


if __name__ == "__main__":
    print("Hello! I'm your NLP-based chatbot. Ask me anything or type 'exit' to quit.")
    while True:
        user_input = input("You: ")
        if user_input.lower() == 'exit':
            print("Chatbot: Goodbye!")
            break
        response = get_response(user_input)
        print(f"Chatbot: {response}")
