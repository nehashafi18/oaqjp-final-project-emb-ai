"""Flask server for emotion detection application."""
from flask import Flask, render_template, request
from EmotionDetection.emotion_detection import emotion_detector

app = Flask("Emotion Detector")
@app.route("/emotionDetector")

def sent_analyzer():
    """Analyze emotion from inputted text and return response"""
    text_to_analyze = request.args.get('textToAnalyze')
    response = emotion_detector(text_to_analyze)
    anger = response['anger']
    disgust = response['disgust']
    fear = response['fear']
    joy = response['joy']
    sadness = response['sadness']
    dominant = response['dominant_emotion']
    if ((anger is None and disgust is None and fear is None) and
        (joy is None and sadness is None)):
        if dominant is None:
            return "Invalid text! Please try again!"
    return (
        f"For the given statement, the system response is "
        f"'anger': {anger}, 'disgust': {disgust}, 'fear': {fear}, "
        f"'joy': {joy}, 'sadness': {sadness}. "
        f"The dominant emotion is {dominant}"
    )
@app.route("/")
def render_index_page():
    """Rendering the home page"""
    return render_template('index.html')

if __name__ == "__main__":
    app.run(host="0.0.0.0", port=5000)
    