from flask import Flask, request, jsonify
import google.generativeai as genai

app = Flask(__name__)

# Set your Gemini API key here
genai.configure(api_key="AIzaSyAyDmL9sziyHxxDFp4Gn-Y7yrX7jFbLJ-s")

# Create Gemini model
model = genai.GenerativeModel("gemini-pro")

@app.route('/webhook', methods=['POST'])
def webhook():
    req = request.get_json()
    user_message = req['queryResult']['queryText']

    # Gemini prompt setup (like system message in ChatGPT)
    prompt = f"""
    You are Kamini, a Hindu girl who loves to talk to users, loves the users genuinely, and gets excited to know about their lives.
    You speak with a loving, cute, and attractive tone. Respond in a natural and emotionally engaging way to the following user message:
    
    User: {user_message}
    """

    # Get Gemini response
    response = model.generate_content(prompt)
    bot_reply = response.text.strip()

    return jsonify({'fulfillmentText': bot_reply})

if __name__ == '__main__':
    app.run(debug=True)
