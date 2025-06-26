# codealpha_tasks-chatbot-for-FAQs 
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>FAQ Chatbot</title>
    <style>
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            margin: 0;
            padding: 0;
            background-color: #f5f5f5;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
        }
        
        .chat-container {
            width: 400px;
            height: 600px;
            background-color: white;
            border-radius: 10px;
            box-shadow: 0 0 20px rgba(0, 0, 0, 0.1);
            display: flex;
            flex-direction: column;
            overflow: hidden;
        }
        
        .chat-header {
            background-color: #4285f4;
            color: white;
            padding: 15px;
            text-align: center;
            font-size: 18px;
            font-weight: bold;
        }
        
        .chat-messages {
            flex: 1;
            padding: 20px;
            overflow-y: auto;
            background-color: #f9f9f9;
        }
        
        .message {
            margin-bottom: 15px;
            max-width: 70%;
            padding: 10px 15px;
            border-radius: 18px;
            line-height: 1.4;
        }
        
        .user-message {
            background-color: #4285f4;
            color: white;
            margin-left: auto;
            border-bottom-right-radius: 5px;
        }
        
        .bot-message {
            background-color: #e0e0e0;
            color: #333;
            margin-right: auto;
            border-bottom-left-radius: 5px;
        }
        
        .chat-input {
            display: flex;
            padding: 15px;
            background-color: white;
            border-top: 1px solid #eee;
        }
        
        #user-input {
            flex: 1;
            padding: 10px;
            border: 1px solid #ddd;
            border-radius: 20px;
            outline: none;
        }
        
        #send-button {
            background-color: #4285f4;
            color: white;
            border: none;
            border-radius: 20px;
            padding: 10px 15px;
            margin-left: 10px;
            cursor: pointer;
        }
        
        #send-button:hover {
            background-color: #3367d6;
        }
    </style>
</head>
<body>
    <div class="chat-container">
        <div class="chat-header">
            FAQ Chatbot
        </div>
        <div class="chat-messages" id="chat-messages">
            <div class="message bot-message">
                Hello! I'm an FAQ chatbot. Ask me any questions about our product or service.
            </div>
        </div>
        <div class="chat-input">
            <input type="text" id="user-input" placeholder="Type your question here...">
            <button id="send-button">Send</button>
        </div>
    </div>

    <script>
        // Sample FAQ data
        const faqs = [
            {
                question: "What is your return policy?",
                answer: "We offer a 30-day return policy for all unused products with original packaging."
            },
            {
                question: "How do I track my order?",
                answer: "You can track your order using the tracking number sent to your email once your order ships."
            },
            {
                question: "What payment methods do you accept?",
                answer: "We accept Visa, Mastercard, American Express, PayPal, and Apple Pay."
            },
            {
                question: "Do you offer international shipping?",
                answer: "Yes, we ship to most countries worldwide. Shipping costs vary by destination."
            },
            {
                question: "How can I contact customer support?",
                answer: "You can reach our support team 24/7 at support@example.com or call us at +1 (555) 123-4567."
            }
        ];

        // Simple text preprocessing
        function preprocessText(text) {
            return text.toLowerCase()
                .replace(/[^\w\s]/g, '') // Remove punctuation
                .replace(/\s+/g, ' ')    // Normalize whitespace
                .trim();
        }

        // Simple similarity matching (will be enhanced later)
        function findBestMatch(userQuestion) {
            const processedUserQuestion = preprocessText(userQuestion);
            
            let bestMatch = null;
            let bestScore = 0;
            
            for (const faq of faqs) {
                const processedFaqQuestion = preprocessText(faq.question);
                
                // Simple word overlap scoring
                const userWords = new Set(processedUserQuestion.split(' '));
                const faqWords = new Set(processedFaqQuestion.split(' '));
                
                let score = 0;
                for (const word of userWords) {
                    if (faqWords.has(word)) {
                        score++;
                    }
                }
                
                if (score > bestScore) {
                    bestScore = score;
                    bestMatch = faq;
                }
            }
            
            return bestMatch || {
                answer: "I'm sorry, I don't have an answer for that question. Please try rephrasing or ask another question."
            };
        }

        // Chat UI functionality
        document.addEventListener('DOMContentLoaded', () => {
            const chatMessages = document.getElementById('chat-messages');
            const userInput = document.getElementById('user-input');
            const sendButton = document.getElementById('send-button');
            
            function addMessage(text, isUser) {
                const messageDiv = document.createElement('div');
                messageDiv.classList.add('message');
                messageDiv.classList.add(isUser ? 'user-message' : 'bot-message');
                messageDiv.textContent = text;
                chatMessages.appendChild(messageDiv);
                chatMessages.scrollTop = chatMessages.scrollHeight;
            }
            
            function handleUserInput() {
                const question = userInput.value.trim();
                if (question) {
                    addMessage(question, true);
                    userInput.value = '';
                    
                    // Simulate thinking
                    setTimeout(() => {
                        const bestMatch = findBestMatch(question);
                        addMessage(bestMatch.answer, false);
                    }, 500);
                }
            }
            
            sendButton.addEventListener('click', handleUserInput);
            userInput.addEventListener('keypress', (e) => {
                if (e.key === 'Enter') {
                    handleUserInput();
                }
            });
        });
    </script>
</body>
</html>


