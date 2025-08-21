<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>QuizMaster - Practice Quiz</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    <style>
        body { font-family: 'Inter', sans-serif; }
        .question-enter { animation: slideIn 0.5s ease-out; }
        @keyframes slideIn {
            from { opacity: 0; transform: translateY(20px); }
            to { opacity: 1; transform: translateY(0); }
        }
        .option-hover:hover { transform: translateY(-2px); transition: all 0.2s ease; }
        .correct { 
            background: linear-gradient(135deg, #10b981, #059669) !important;
            border-color: #059669 !important;
            color: white !important;
            transform: scale(1.02);
            box-shadow: 0 4px 15px rgba(16, 185, 129, 0.3);
        }
        .correct * {
            color: white !important;
        }
        .correct span:first-child {
            background: rgba(255,255,255,0.9) !important;
            color: #059669 !important;
        }
        .incorrect { 
            background: linear-gradient(135deg, #ef4444, #dc2626) !important;
            border-color: #dc2626 !important;
            color: white !important;
            transform: scale(0.98);
            box-shadow: 0 4px 15px rgba(239, 68, 68, 0.3);
        }
        .incorrect * {
            color: white !important;
        }
        .incorrect span:first-child {
            background: rgba(255,255,255,0.9) !important;
            color: #dc2626 !important;
        }
        .selected { border-color: #3b82f6; background-color: #eff6ff; }
    </style>
</head>
<body class="bg-gradient-to-br from-blue-50 to-indigo-100 min-h-screen">
    <div class="container mx-auto px-4 py-8 max-w-4xl">
        <!-- Header -->
        <div class="text-center mb-8">
            <h1 class="text-4xl font-bold text-gray-800 mb-2">QuizMaster</h1>
            <p class="text-gray-600">Test your knowledge with our practice quiz</p>
        </div>

        <!-- Progress Bar -->
        <div class="bg-white rounded-lg shadow-sm p-4 mb-6">
            <div class="flex justify-between items-center mb-2">
                <span class="text-sm font-medium text-gray-600">Progress</span>
                <span class="text-sm font-medium text-gray-600" id="progress-text">1 of 65</span>
            </div>
            <div class="w-full bg-gray-200 rounded-full h-2">
                <div class="bg-blue-500 h-2 rounded-full transition-all duration-300" id="progress-bar" style="width: 1.5385%"></div>
            </div>
        </div>

        <!-- Quiz Card -->
        <div class="bg-white rounded-xl shadow-lg p-8 mb-6" id="quiz-card">
            <div class="question-enter">
                <div class="mb-6">
                    <span class="bg-blue-100 text-blue-800 text-xs font-medium px-2.5 py-0.5 rounded-full" id="q-number">Question 1</span>
                </div>
                
                <h2 class="text-2xl font-semibold text-gray-800 mb-8 leading-relaxed" id="question-text">
                    Loading...
                </h2>

                <div class="space-y-4" id="options-container">
                    <div class="option-hover cursor-pointer p-4 border-2 border-gray-200 rounded-lg transition-all duration-200 hover:border-blue-300 hover:shadow-md" data-option="0">
                        <div class="flex items-center">
                            <span class="w-8 h-8 bg-gray-100 text-gray-600 rounded-full flex items-center justify-center font-medium mr-4">A</span>
                            <span class="text-gray-700 font-medium">Option A</span>
                        </div>
                    </div>
                    
                    <div class="option-hover cursor-pointer p-4 border-2 border-gray-200 rounded-lg transition-all duration-200 hover:border-blue-300 hover:shadow-md" data-option="1">
                        <div class="flex items-center">
                            <span class="w-8 h-8 bg-gray-100 text-gray-600 rounded-full flex items-center justify-center font-medium mr-4">B</span>
                            <span class="text-gray-700 font-medium">Option B</span>
                        </div>
                    </div>
                    
                    <div class="option-hover cursor-pointer p-4 border-2 border-gray-200 rounded-lg transition-all duration-200 hover:border-blue-300 hover:shadow-md" data-option="2">
                        <div class="flex items-center">
                            <span class="w-8 h-8 bg-gray-100 text-gray-600 rounded-full flex items-center justify-center font-medium mr-4">C</span>
                            <span class="text-gray-700 font-medium">Option C</span>
                        </div>
                    </div>
                    
                    <div class="option-hover cursor-pointer p-4 border-2 border-gray-200 rounded-lg transition-all duration-200 hover:border-blue-300 hover:shadow-md" data-option="3">
                        <div class="flex items-center">
                            <span class="w-8 h-8 bg-gray-100 text-gray-600 rounded-full flex items-center justify-center font-medium mr-4">D</span>
                            <span class="text-gray-700 font-medium">Option D</span>
                        </div>
                    </div>
                </div>
            </div>
        </div>

        <!-- Navigation -->
        <div class="flex justify-between items-center">
            <button id="prev-btn" class="px-6 py-3 bg-gray-200 text-gray-600 rounded-lg font-medium hover:bg-gray-300 transition-colors disabled:opacity-50 disabled:cursor-not-allowed" disabled>
                Previous
            </button>
            
            <div class="text-center">
                <div class="text-sm text-gray-600 mb-1">Score</div>
                <div class="text-2xl font-bold text-blue-600" id="score">0/0</div>
            </div>
            
            <button id="next-btn" class="px-6 py-3 bg-blue-500 text-white rounded-lg font-medium hover:bg-blue-600 transition-colors disabled:opacity-50 disabled:cursor-not-allowed" disabled>
                Next
            </button>
        </div>

        <!-- Results Modal -->
        <div id="results-modal" class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center z-50 hidden">
            <div class="bg-white rounded-xl shadow-2xl p-8 max-w-md w-full mx-4">
                <div class="text-center">
                    <div class="w-16 h-16 bg-green-100 rounded-full flex items-center justify-center mx-auto mb-4">
                        <svg class="w-8 h-8 text-green-500" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7"></path>
                        </svg>
                    </div>
                    <h3 class="text-2xl font-bold text-gray-800 mb-2">Quiz Complete!</h3>
                    <p class="text-gray-600 mb-6">Great job on completing the quiz</p>
                    <div class="text-4xl font-bold text-blue-600 mb-6" id="final-score">0/65</div>
                    <button id="restart-btn" class="w-full px-6 py-3 bg-blue-500 text-white rounded-lg font-medium hover:bg-blue-600 transition-colors">
                        Try Again
                    </button>
                </div>
            </div>
        </div>
    </div>

    <script type="module">
        const quizData = [
            /* 1 - 65 (sequencewise) */
            { question: "1. भारतीय संविधान की आठवीं अनुसूची में कौन-सी भाषाएँ बाद में जोड़ी गई थीं?", options: ["अंग्रेजी, सिंधी, मराठी, संस्कृत","संस्कृत, सिंधी, कोंकणी, मणिपुरी","सिंधी, कोंकणी, मणिपुरी, नेपाली","मराठी, उड़िया, कोंकणी, नेपाली"], correct: 2 },
            { question: "2. निम्नलिखित में कौन सा एक सही सुमेलित नहीं है?", options: ["आठवीं अनुसूची: भाषाएँ","दूसरी अनुसूची: शपथ प्रारूप","चौथी अनुसूची: राज्य सभा में स्थानों का आवंटन","दसवीं अनुसूची: दल-बदल संबंधी प्रावधान"], correct: 1 },
            { question: "3. भारतीय संविधान का 100वाँ संशोधन प्रदान करता है", options: ["जीविका की सुरक्षा और फेरीवालों का विनियमन","भारत द्वारा क्षेत्रों को प्राप्त करना और बांग्लादेश में कुछ क्षेत्रों का हस्तांतरण करना","राज्यपालों के पारिश्रमिक, भत्तों और विशेष अधिकारों","आंध्र प्रदेश के पुनर्गठन"], correct: 1 },
            { question: "4. भारतीय संविधान के किस अनुच्छेद के अंतर्गत जिला योजना समिति का गठन होता है?", options: ["अनुच्छेद 243 ZD","अनुच्छेद 244 ZD","अनुच्छेद 242 ZD","अनुच्छेद 243 ZE"], correct: 0 },
            { question: "5. भारत के राज्यों और केंद्रशासित प्रदेशों को संविधान की किस अनुसूची में सूचीबद्ध किया गया है?", options: ["पहली अनुसूची","दूसरी अनुसूची","तीसरी अनुसूची","चौथी अनुसूची"], correct: 0 },
            { question: "6. भारत के संविधान में प्रेस की स्वतंत्रता किस अनुच्छेद के तहत सुनिश्चित है?", options: ["अनुच्छेद 15","अनुच्छेद 17","अनुच्छेद 19","अनुच्छेद 21"], correct: 2 },
            { question: "7. निम्न में से भारत में किसका संशोधन विशेष बहुमत से ही किया जा सकता है?", options: ["नए राज्य का गठन","सांसदों के वेतन-भत्ते","राष्ट्रपति के भत्ते","अनुच्छेद 368 के द्वारा संविधान का संशोधन"], correct: 3 },
            { question: "8. 'उपाधियों के अंत' से संबंधित अनुच्छेद है", options: ["अनुच्छेद 51","अनुच्छेद 50","अनुच्छेद 18","अनुच्छेद 32"], correct: 2 },
            { question: "9. डॉ. अम्बेडकर के अनुसार भारतीय संविधान का सबसे महत्वपूर्ण अनुच्छेद है", options: ["अनुच्छेद 21","अनुच्छेद 24","अनुच्छेद 32","अनुच्छेद 256"], correct: 2 },
            { question: "10. निम्न में से संविधान के कौन-से अनुच्छेद आपातकाल से संबंधित हैं?", options: ["केवल I (अनुच्छेद 352)","I तथा III दोनों (352, 360)","I तथा II दोनों (352, 356)","I, II तथा III सभी (352, 356, 360)"], correct: 3 },

            { question: "11. किस अनुच्छेद के अंतर्गत राज्य में संवैधानिक तंत्र विफल होने पर आपातकाल (राष्ट्रपति शासन) लगाया जा सकता है?", options: ["अनुच्छेद 350","अनुच्छेद 352","अनुच्छेद 356","अनुच्छेद 368"], correct: 2 },
            { question: "12. 73वें संशोधन अधिनियम द्वारा कौन-सी अनुसूची जोड़ी गई?", options: ["आठवीं अनुसूची","नौवीं अनुसूची","दसवीं अनुसूची","ग्यारहवीं अनुसूची"], correct: 3 },
            { question: "13. अनुच्छेद 360 किसे वित्तीय आपातकाल घोषित करने का अधिकार देता है?", options: ["भारत के वित्त मंत्री","भारतीय रिजर्व बैंक के गवर्नर","भारत के राष्ट्रपति","भारत के रक्षा मंत्री"], correct: 2 },
            { question: "14. संविधान में तीन सूचियाँ—संघ सूची, राज्य सूची तथा", options: ["जिला सूची","पंचायत सूची","समवर्ती सूची","कोई विकल्प सही नहीं"], correct: 2 },
            { question: "15. अनुच्छेद 36 से 51 मुख्यतः राज्य के नीति निर्देशक तत्व हैं, जो संविधान के किस भाग में दिए हैं?", options: ["भाग II","भाग III","भाग IV","भाग V"], correct: 2 },
            { question: "16. जब भारतीय संविधान अपनाया गया, तब उसमें कितनी अनुसूचियाँ थीं?", options: ["8","10","12","14"], correct: 0 },
            { question: "17. किस अनुच्छेद के अनुसार उच्च न्यायालय के न्यायाधीश की नियुक्ति राष्ट्रपति, भारत के मुख्य न्यायाधीश व राज्यपाल की सलाह से की जाती है?", options: ["अनुच्छेद 21","अनुच्छेद 201","अनुच्छेद 217","अनुच्छेद 72"], correct: 2 },
            { question: "18. आर्थिक रूप से कमजोर वर्ग (EWS) के 10% आरक्षण का प्रावधान किस संशोधन से जोड़ा गया?", options: ["100वाँ","103वाँ","99वाँ","143वाँ"], correct: 1 },
            { question: "19. नागालैंड भारतीय संघ का भाग किस संशोधन द्वारा (1963) बना?", options: ["17वाँ","13वाँ","16वाँ","14वाँ"], correct: 1 },
            { question: "20. 124वाँ संशोधन विधेयक, 2019 किससे संबंधित है?", options: ["त्रिपल तलाक","एयर इंडिया का निजीकरण","जीएसटी","आर्थिक आरक्षण"], correct: 3 },

            { question: "21. किस अनुच्छेद में लोकसभा में अनुसूचित जाति/जनजाति के लिए सीट आरक्षण का प्रावधान है?", options: ["अनुच्छेद 325","अनुच्छेद 321","अनुच्छेद 330","अनुच्छेद 335"], correct: 2 },
            { question: "22. किस अनुसूची में विभिन्न संवैधानिक पदाधिकारियों की शपथ का प्रारूप दिया है?", options: ["चौथी अनुसूची","पाँचवीं अनुसूची","तीसरी अनुसूची","दूसरी अनुसूची"], correct: 2 },
            { question: "23. किस अनुच्छेद के तहत धर्म, वंश, जाति, लिंग, जन्म स्थान के आधार पर भेदभाव पर रोक है?", options: ["अनुच्छेद 10","अनुच्छेद 12","अनुच्छेद 13","अनुच्छेद 15"], correct: 3 },
            { question: "24. 10 मार्च 2019 तक, भारतीय संविधान में कुल कितने संशोधन हुए थे?", options: ["99","101","105","103"], correct: 3 },
            { question: "25. किस अनुच्छेद के तहत ‘स्वास्थ्य का अधिकार’ संरक्षित माना गया है?", options: ["अनुच्छेद 19","अनुच्छेद 20","अनुच्छेद 21-A","अनुच्छेद 21"], correct: 3 },
            { question: "26. किस अनुच्छेद में कहा गया है कि 'भारत के क्षेत्र में सभी प्राधिकरण, नागरिक व न्यायिक, सर्वोच्च न्यायालय के सहायक के रूप में कार्य करेंगे'?", options: ["अनुच्छेद 137","अनुच्छेद 121","अनुच्छेद 144","अनुच्छेद 157"], correct: 2 },
            { question: "27. 'पशुधन एवं पशुपालन' विषय किस सूची में आता है?", options: ["समवर्ती सूची","राज्य सूची","संघ सूची","अवशिष्ट सूची"], correct: 1 },
            { question: "28. संविधान (86वाँ संशोधन) अधिनियम, 2002 के संबंध में कौन-सा कथन सही नहीं है?", options: ["अनुच्छेद 45 के अंतर्गत 6 वर्ष से कम आयु के बच्चों हेतु प्रारंभिक देखभाल/शिक्षा का प्रावधान किया।","अनुच्छेद 51A के तहत 6–14 वर्ष के बच्चों/आश्रित को शिक्षा का अवसर देना माता-पिता का मौलिक कर्तव्य बनाया।","अनुच्छेद 75 के अंतर्गत इन प्रावधानों के क्रियान्वयन हेतु केंद्र में एक नोडल मंत्रालय स्थापित करना बाध्यकारी बनाया।","अनुच्छेद 21A के तहत 6–14 वर्ष के सभी बच्चों हेतु निःशुल्क एवं अनिवार्य शिक्षा को मौलिक अधिकार बनाया।"], correct: 2 },
            { question: "29. अनुच्छेद 361 के अनुसार किसके कार्यकाल के दौरान उनके विरुद्ध आपराधिक कार्यवाही आरम्भ नहीं की जा सकती?", options: ["उपराष्ट्रपति","प्रधानमंत्री","मुख्यमंत्री","राज्यपाल"], correct: 3 },

            { question: "30. भारतीय प्रशासनिक सेवा और भारतीय पुलिस सेवा को किस अनुच्छेद के तहत संसद द्वारा बनाई गई सेवाओं के अंतर्गत माना जाता है?", options: ["अनुच्छेद 307","अनुच्छेद 292","अनुच्छेद 301","अनुच्छेद 312"], correct: 3 },
            { question: "31. भारत के संविधान का अनुच्छेद 17 किसके उन्मूलन से संबंधित है?", options: ["सती प्रथा","अस्पृश्यता","उपाधियाँ","दास प्रथा"], correct: 1 },
            { question: "32. किस अनुच्छेद में धर्म, वंश, जाति, लिंग, जन्म-स्थान के आधार पर भेदभाव पर रोक है?", options: ["अनुच्छेद 23","अनुच्छेद 25","अनुच्छेद 19","अनुच्छेद 15"], correct: 3 },
            { question: "33. किस अनुच्छेद में किसी जनसमूह द्वारा बोली जाने वाली भाषा के संबंध में विशेष प्रावधान किए गए हैं?", options: ["अनुच्छेद 347","अनुच्छेद 357","अनुच्छेद 337","अनुच्छेद 374"], correct: 0 },

            { question: "34. किस संशोधन द्वारा व्यवस्था की गई कि चुनाव के बाद किसी अन्य राजनीतिक दल में शामिल होना अवैध माना जाएगा (दल-बदल)?", options: ["86वाँ","61वाँ","52वाँ","92वाँ"], correct: 2 },
            { question: "35. कौन-सा अनुच्छेद सर्वोच्च न्यायालय की अनुषंगी शक्तियों से संबंधित है?", options: ["अनुच्छेद 143","अनुच्छेद 140","अनुच्छेद 138","अनुच्छेद 150"], correct: 1 },
            { question: "36. किस अनुसूची का मिलान उसकी सामग्री के साथ गलत है?", options: ["पहली—राज्य और केंद्रशासित प्रदेश","दूसरी—भाषाएँ","चौथी—राज्य परिषद (राज्यसभा) में सीटों का आवंटन","तीसरी—शपथ/अभिवचन के स्वरूप"], correct: 1 },
            { question: "37. किस संशोधन के अंतर्गत वस्तु एवं सेवा कर (GST) लगाया गया?", options: ["97वाँ","99वाँ","103वाँ","101वाँ"], correct: 3 },
            { question: "38. कौन-सा अनुच्छेद अनुसूचित क्षेत्रों और अनुसूचित जनजातियों के प्रशासन/नियंत्रण से संबंधित प्रावधानों का उल्लेख करता है?", options: ["अनुच्छेद 244(1)","अनुच्छेद 244(2)","अनुच्छेद 222(1)","अनुच्छेद 222(2)"], correct: 0 },
            { question: "39. किस अनुच्छेद के तहत राज्य, राष्ट्रीय महत्त्व/ऐतिहासिक हित से संबंधित प्रत्येक स्मारक, स्थान और वस्तु का संरक्षण करने को बाध्य है?", options: ["अनुच्छेद 49","अनुच्छेद 48","अनुच्छेद 51A(f)","अनुच्छेद 47"], correct: 0 },

            { question: "40. प्रशासनिक न्यायाधिकरणों से संबंधित अनुच्छेद है", options: ["अनुच्छेद 323A","अनुच्छेद 271","अनुच्छेद 290A","अनुच्छेद 307"], correct: 0 },
            { question: "41. किस संशोधन द्वारा पहली अनुसूची में संशोधन कर गोवा, दमन और दीव को भारत के आठवें केन्द्रशासित प्रदेश के रूप में शामिल किया गया?", options: ["13वाँ संशोधन","14वाँ संशोधन","12वाँ संशोधन","11वाँ संशोधन"], correct: 2 },
            { question: "42. 6–14 वर्ष के सभी बच्चों की निःशुल्क व अनिवार्य शिक्षा से संबंधित अनुच्छेद है", options: ["अनुच्छेद 74","अनुच्छेद 21A","अनुच्छेद 101","अनुच्छेद 31A"], correct: 1 },
            { question: "43. कारखानों आदि में बच्चों से काम करवाने पर प्रतिबंध लगाने वाला अनुच्छेद है", options: ["अनुच्छेद 21","अनुच्छेद 24","अनुच्छेद 31","अनुच्छेद 17"], correct: 1 },
            { question: "44. किसके अंतर्गत आईएएस/आईपीएस को संसद द्वारा सृजित सेवाएँ माना जाता है?", options: ["अनुच्छेद 312 के अंतर्गत","अनुच्छेद 301 के अंतर्गत","अनुच्छेद 292 के अंतर्गत","अनुच्छेद 307 के अंतर्गत"], correct: 0 },
            { question: "45. संसद के दोनों सदनों की संयुक्त बैठक का प्रावधान किस अनुच्छेद में है?", options: ["अनुच्छेद 93","अनुच्छेद 126","अनुच्छेद 108","अनुच्छेद 122"], correct: 2 },
            { question: "46. जी.एस.टी. परिषद की स्थापना से संबंधित अनुच्छेद है", options: ["अनुच्छेद 246A","अनुच्छेद 279A","अनुच्छेद 269A","अनुच्छेद 323A"], correct: 1 },
            { question: "47. किस संशोधन ने घोषित किया कि संसद के पास अनुच्छेद 368 के तहत किसी भी मौलिक अधिकार को कम/हटाने की शक्ति है और ऐसा अधिनियम अनुच्छेद 13 के तहत ‘कानून’ नहीं होगा?", options: ["23वाँ संशोधन","20वाँ संशोधन","24वाँ संशोधन","28वाँ संशोधन"], correct: 2 },
            { question: "48. भारतीय संविधान के 100वें संशोधन को मंजूरी किस राष्ट्रपति ने दी थी?", options: ["प्रणब मुखर्जी","रामनाथ कोविंद","ए. पी. जे. अब्दुल कलाम","प्रतिभा देवीसिंह पाटिल"], correct: 0 },

            { question: "49. राष्ट्रीय राजधानी क्षेत्र दिल्ली सरकार (संशोधन) विधेयक, 2021 द्वारा किस वर्ष के अधिनियम में संशोधन किया गया?", options: ["1998","1994","1996","1991"], correct: 3 },
            { question: "50. कौन-सा संशोधन ग्राम सभा/पंचायती राज प्रणाली की स्थापना के रूप में परिकल्पित करता है?", options: ["71वाँ","72वाँ","73वाँ","74वाँ"], correct: 2 },
            { question: "51. भाषा के आधार पर राज्यों के पुनर्गठन को संवैधानिक रूप से बदलने के प्राथमिक उद्देश्य से पारित संशोधन कौन-सा था?", options: ["10वाँ संशोधन","6वाँ संशोधन","7वाँ संशोधन","4था संशोधन"], correct: 2 },
            { question: "52. किस संशोधन अधिनियम ने पूर्व शासकों के प्रिवी पर्स व विशेषाधिकार समाप्त कर दिए?", options: ["25वाँ (1971)","26वाँ (1971)","28वाँ (1972)","27वाँ (1971)"], correct: 1 },
            { question: "53. नगरपालिकाओं को संवैधानिक मान्यता देने हेतु 1993 में कौन-सा संशोधन किया गया?", options: ["73वाँ CAA, 1990","72वाँ CAA, 1989","74वाँ CAA, 1992","71वाँ CAA, 1988"], correct: 2 },
            { question: "54. संघ कार्यपालिका से संबंधित अनुच्छेद हैं", options: ["अनुच्छेद 38–50","अनुच्छेद 52–78","अनुच्छेद 80–86","अनुच्छेद 112–118"], correct: 1 },
            { question: "55. अनुच्छेद 32 को भारतीय संविधान का ‘हृदय और आत्मा’ किसने कहा?", options: ["एम. बी. पायली","बी. आर. अम्बेडकर","जवाहरलाल नेहरू","सच्चिदानंद सिन्हा"], correct: 1 },
            { question: "56. संविधान संशोधन प्रक्रिया किस अनुच्छेद में है?", options: ["अनुच्छेद 332","अनुच्छेद 363","अनुच्छेद 368","अनुच्छेद 360"], correct: 2 },
            { question: "57. कौन-सी अनुसूची संघ, राज्य व समवर्ती सूची के संदर्भ में शक्तियों का बँटवारा करती है?", options: ["पाँचवीं अनुसूची","नौवीं अनुसूची","तीसरी अनुसूची","सातवीं अनुसूची"], correct: 3 },
            { question: "58. राष्ट्रपति के महाभियोग से संबंधित अनुच्छेद है", options: ["अनुच्छेद 53","अनुच्छेद 58","अनुच्छेद 61","अनुच्छेद 59"], correct: 2 },
            { question: "59. संविधान (अनुसूचित जाति) आदेश संशोधन विधेयक, 2021 ने किस वर्ष के आदेश को संशोधित किया?", options: ["1952","1950","1947","1959"], correct: 1 },
            { question: "60. 1978 में किस संशोधन द्वारा संपत्ति के अधिकार को मौलिक अधिकारों की सूची से हटाया गया?", options: ["44वाँ संशोधन","45वाँ संशोधन","46वाँ संशोधन","43वाँ संशोधन"], correct: 0 },
            { question: "61. धन विधेयक की परिभाषा किस अनुच्छेद में है?", options: ["अनुच्छेद 120","अनुच्छेद 100","अनुच्छेद 115","अनुच्छेद 110"], correct: 3 },
            { question: "62. 'अल्पसंख्यकों को अपनी रूचि के शिक्षण संस्थान स्थापित करने व संचालित करने का अधिकार' किस अनुच्छेद से सुनिश्चित है?", options: ["अनुच्छेद 25","अनुच्छेद 28","अनुच्छेद 36","अनुच्छेद 30"], correct: 3 },
            { question: "63. उपराष्ट्रपति के चुनाव से संबंधित अनुच्छेद है", options: ["अनुच्छेद 66","अनुच्छेद 68","अनुच्छेद 64","अनुच्छेद 62"], correct: 0 },
            { question: "64. उच्च न्यायालय को मौलिक अधिकारों के प्रवर्तन हेतु रिट जारी करने का अधिकार किस अनुच्छेद से है?", options: ["अनुच्छेद 226","अनुच्छेद 235","अनुच्छेद 230","अनुच्छेद 242"], correct: 0 },
            { question: "65. 'जीवन और व्यक्तिगत स्वतंत्रता का संरक्षण' किस अनुच्छेद में है?", options: ["अनुच्छेद 21","अनुच्छेद 23","अनुच्छेद 19","अनुच्छेद 20"], correct: 0 }
        ];

        let currentQuestion = 0;
        let score = 0;
        let selectedOption = null;
        let answered = false;

        const questionText = document.getElementById('question-text');
        const optionsContainer = document.getElementById('options-container');
        const prevBtn = document.getElementById('prev-btn');
        const nextBtn = document.getElementById('next-btn');
        const progressBar = document.getElementById('progress-bar');
        const progressText = document.getElementById('progress-text');
        const scoreDisplay = document.getElementById('score');
        const resultsModal = document.getElementById('results-modal');
        const finalScore = document.getElementById('final-score');
        const restartBtn = document.getElementById('restart-btn');
        const qNumber = document.getElementById('q-number');

        function loadQuestion() {
            const question = quizData[currentQuestion];
            questionText.textContent = question.question;
            
            const options = optionsContainer.children;
            for (let i = 0; i < options.length; i++) {
                const option = options[i];
                const span = option.querySelector('span:last-child');
                span.textContent = question.options[i] || "";
                
                // Reset styling
                option.className = 'option-hover cursor-pointer p-4 border-2 border-gray-200 rounded-lg transition-all duration-200 hover:border-blue-300 hover:shadow-md';
                option.querySelector('span:first-child').className = 'w-8 h-8 bg-gray-100 text-gray-600 rounded-full flex items-center justify-center font-medium mr-4';
            }
            
            // Update progress
            progressText.textContent = `${currentQuestion + 1} of ${quizData.length}`;
            progressBar.style.width = `${((currentQuestion + 1) / quizData.length) * 100}%`;
            qNumber.textContent = `Question ${currentQuestion + 1}`;
            
            // Update navigation
            prevBtn.disabled = currentQuestion === 0;
            nextBtn.disabled = true;
            nextBtn.textContent = currentQuestion === quizData.length - 1 ? 'Finish' : 'Next';
            
            selectedOption = null;
            answered = false;
            
            // Add animation class
            const qEnter = document.querySelector('.question-enter');
            if (qEnter) {
                qEnter.style.animation = 'none';
                setTimeout(() => { qEnter.style.animation = 'slideIn 0.5s ease-out'; }, 10);
            }

            // Update score display (progressive)
            scoreDisplay.textContent = `${score}/${currentQuestion}`;
        }

        function selectOption(optionIndex) {
            if (answered) return;
            
            selectedOption = optionIndex;
            answered = true;
            
            // Show answer immediately
            showAnswer();
            
            // Enable next button
            nextBtn.disabled = false;
            nextBtn.focus();
        }

        function showAnswer() {
            answered = true;
            const question = quizData[currentQuestion];
            const options = optionsContainer.children;
            
            for (let i = 0; i < options.length; i++) {
                const option = options[i];
                const circle = option.querySelector('span:first-child');
                const textSpan = option.querySelector('span:last-child');
                
                if (i === question.correct) {
                    option.className = 'correct p-4 border-2 rounded-lg transition-all duration-300';
                    circle.className = 'w-8 h-8 rounded-full flex items-center justify-center font-medium mr-4';
                    textSpan.className = 'font-medium';
                } else if (i === selectedOption) {
                    option.className = 'incorrect p-4 border-2 rounded-lg transition-all duration-300';
                    circle.className = 'w-8 h-8 rounded-full flex items-center justify-center font-medium mr-4';
                    textSpan.className = 'font-medium';
                } else {
                    option.className = 'p-4 border-2 border-gray-300 rounded-lg opacity-50 transition-all duration-300';
                }
            }
            
            if (selectedOption === question.correct) {
                score++;
            }
            
            updateScore();
        }

        function updateScore() {
            scoreDisplay.textContent = `${score}/${currentQuestion + 1}`;
        }

        function showResults() {
            finalScore.textContent = `${score}/${quizData.length}`;
            resultsModal.classList.remove('hidden');
        }

        function restartQuiz() {
            currentQuestion = 0;
            score = 0;
            selectedOption = null;
            answered = false;
            resultsModal.classList.add('hidden');
            scoreDisplay.textContent = '0/0';
            loadQuestion();
        }

        // Event listeners
        optionsContainer.addEventListener('click', (e) => {
            const option = e.target.closest('[data-option]');
            if (option && !answered) {
                const optionIndex = Array.from(optionsContainer.children).indexOf(option);
                selectOption(optionIndex);
            }
        });

        nextBtn.addEventListener('click', () => {
            if (!answered) return;
            if (currentQuestion < quizData.length - 1) {
                currentQuestion++;
                loadQuestion();
            } else {
                showResults();
            }
        });

        prevBtn.addEventListener('click', () => {
            if (currentQuestion > 0) {
                // if user goes back, do NOT change score (we keep simple state — answers are not stored for revisit)
                currentQuestion--;
                loadQuestion();
            }
        });

        restartBtn.addEventListener('click', restartQuiz);

        // Initialize
        loadQuestion();
    </script>
</body>
</html>
