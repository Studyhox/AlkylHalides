<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Interactive Guide to Beta-Carbon Effects on SN2 Reactions</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
    <!-- Chosen Palette: Warm Neutral Harmony -->
    <!-- Application Structure Plan: The application is designed as a guided, single-page educational module. It uses a tab-based navigation to break down the complex topic into digestible sections: 1) Fundamentals (SN2 basics), 2) The Core Problem (Beta-Carbon Hindrance), 3) Case Studies (Neopentyl & Cyclohexyl examples), and 4) Competing Reactions (SN2 vs. E2). This thematic structure allows a user to build foundational knowledge before diving into specific, illustrative examples, which is more effective for learning than a linear report format. The interactive rate chart and diagrams are placed within the relevant case study to provide immediate visual context to the text. The new Gemini-powered tutor is a standalone section at the bottom, acting as an additional resource for deeper, personalized inquiry. -->
    <!-- Visualization & Content Choices: 
        - Report Info: Relative SN2 reaction rates for various alkyl halides. Goal: Compare the drastic rate differences. Viz/Method: Interactive Bar Chart (Chart.js). Interaction: Hovering over bars reveals precise relative rates. Justification: A bar chart provides an immediate, powerful visual comparison of magnitudes, making the "100,000 times slower" concept instantly understandable. Chart.js is used for its responsiveness and ease of implementation.
        - Report Info: Steric hindrance in cyclohexyl systems (axial vs. equatorial). Goal: Organize and explain spatial relationships. Viz/Method: HTML/CSS Diagram. Interaction: Static visual comparison. Justification: A simple, clean diagram built with HTML/CSS is sufficient to illustrate the accessibility of the backside attack path for axial vs. equatorial positions. It avoids the overhead of a graphics library and is clearer than a purely textual description.
        - Report Info: General concepts and explanations. Goal: Inform. Viz/Method: Structured text blocks with highlighting. Interaction: Content revealed/hidden based on tab selection. Justification: Grouping text into thematic, collapsible sections prevents overwhelming the user and allows them to focus on one concept at a time.
        - Report Info: User questions about SN2/E2. Goal: Provide personalized, dynamic explanations. Viz/Method: Gemini API for text generation. Interaction: User types a question, clicks a button, and receives a generated response. Justification: This transforms the app into a collaborative, educational tool, providing a personal tutor experience that goes beyond the static content of the report. It makes the learning experience more engaging and effective.
        - CONFIRMATION: NO SVG graphics used. NO Mermaid JS used. -->
    <style>
        body {
            font-family: 'Inter', sans-serif;
            background-color: #FDFBF8; /* Warm off-white */
            color: #4A4A4A;
        }
        .tab-active {
            background-color: #8D7B68; /* Muted brown */
            color: #FDFBF8;
            border-color: #8D7B68;
        }
        .tab-inactive {
            background-color: #FDFBF8;
            color: #8D7B68;
            border-color: #C8B6A6;
        }
        .content-card {
            background-color: #FFFFFF;
            border: 1px solid #EAEAEA;
            border-radius: 0.75rem;
            box-shadow: 0 4px 6px -1px rgb(0 0 0 / 0.05), 0 2px 4px -2px rgb(0 0 0 / 0.05);
        }
        .chart-container {
            position: relative;
            width: 100%;
            max-width: 700px;
            margin-left: auto;
            margin-right: auto;
            height: 300px;
            max-height: 40vh;
        }
        @media (min-width: 768px) {
            .chart-container {
                height: 400px;
            }
        }
        .diagram-box {
            border: 2px dashed #C8B6A6;
            background-color: #FDFBF8;
        }
        .arrow {
            font-size: 2rem;
            line-height: 1;
            color: #A4907C;
        }
        .loading-spinner {
            border: 4px solid #f3f3f3;
            border-top: 4px solid #8D7B68;
            border-radius: 50%;
            width: 24px;
            height: 24px;
            animation: spin 1s linear infinite;
        }
        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }
    </style>
</head>
<body class="antialiased">

    <div class="container mx-auto px-4 py-8 md:py-12">
        
        <header class="text-center mb-10">
            <h1 class="text-4xl md:text-5xl font-bold text-[#65451F]">The Influence of Beta-Carbon Substitution on Sₙ2 Reactions</h1>
            <p class="mt-4 text-lg text-gray-600 max-w-3xl mx-auto">An interactive exploration of how steric hindrance beyond the reaction center dictates chemical reactivity.</p>
        </header>

        <nav class="flex flex-wrap justify-center gap-2 md:gap-4 mb-8">
            <button class="tab tab-active" onclick="showTab('fundamentals')">1. Sₙ2 Fundamentals</button>
            <button class="tab tab-inactive" onclick="showTab('problem')">2. The β-Carbon Problem</button>
            <button class="tab tab-inactive" onclick="showTab('cases')">3. Case Studies</button>
            <button class="tab tab-inactive" onclick="showTab('competition')">4. Sₙ2 vs E2 Competition</button>
        </nav>

        <main>
            <div id="fundamentals" class="tab-content">
                <div class="content-card p-6 md:p-8">
                    <h2 class="text-2xl font-bold text-[#8D7B68] mb-4">Mechanism and Steric Effects</h2>
                    <div class="space-y-4 text-base md:text-lg leading-relaxed">
                        <p>The Sₙ2 (Substitution, Nucleophilic, Bimolecular) reaction is a cornerstone of organic chemistry. Its rate is determined by a single, concerted step where a nucleophile attacks the carbon atom bearing a leaving group. This attack occurs from the "backside," 180° opposite to the leaving group, leading to an inversion of stereochemistry. The reaction's speed is critically sensitive to <strong class="font-semibold text-[#65451F]">steric hindrance</strong>—the physical blocking of this backside attack path by bulky atomic groups.</p>
                        <p>This sensitivity explains the standard reactivity trend based on the substitution at the alpha-carbon (the one with the leaving group): <strong class="font-semibold">Methyl > Primary > Secondary</strong>. Tertiary halides are generally unreactive via the Sₙ2 pathway because the three bulky groups directly on the alpha-carbon make the backside attack impossible. This application explores a more nuanced aspect: how bulkiness on the <strong class="font-semibold">beta-carbon</strong> (the carbon adjacent to the alpha-carbon) can also have a dramatic, and sometimes surprising, effect on the reaction rate.</p>
                    </div>
                </div>
            </div>

            <div id="problem" class="tab-content hidden">
                <div class="content-card p-6 md:p-8">
                    <h2 class="text-2xl font-bold text-[#8D7B68] mb-4">The High-Energy Transition State</h2>
                    <div class="space-y-4 text-base md:text-lg leading-relaxed">
                        <p>The Sₙ2 reaction proceeds through a high-energy <strong class="font-semibold text-[#65451F]">trigonal bipyramidal transition state</strong>. In this fleeting moment, the central carbon is partially bonded to five groups: the incoming nucleophile, the departing leaving group, and the three other substituents. Any steric crowding, whether from groups on the alpha or beta carbons, forces atoms uncomfortably close together. This creates van der Waals repulsion, which <strong class="font-semibold">destabilizes the transition state</strong>, increases the reaction's activation energy, and dramatically slows the rate.</p>
                        <p>Therefore, even if the alpha-carbon is primary and seems accessible, large groups on the beta-carbon can act like a <strong class="font-semibold">steric shield</strong>, blocking the nucleophile's trajectory. This is why a simple count of alpha-substituents is not enough to predict Sₙ2 reactivity; the entire 3D environment of the substrate must be considered.</p>
                    </div>
                </div>
            </div>

            <div id="cases" class="tab-content hidden">
                <div class="grid grid-cols-1 lg:grid-cols-2 gap-8">
                    <div class="content-card p-6 md:p-8">
                        <h3 class="text-2xl font-bold text-[#8D7B68] mb-4">Case 1: The Neopentyl Anomaly</h3>
                        <p class="mb-6 text-base md:text-lg leading-relaxed">Neopentyl halides are primary alkyl halides, yet they are virtually inert to Sₙ2 reactions. This is because their beta-carbon is <strong class="font-semibold text-[#65451F]">quaternary</strong> (a *tert*-butyl group). The three methyl groups on the beta-carbon form an impassable steric barrier, completely blocking the nucleophile's backside approach. The chart below visualizes this dramatic rate drop.</p>
                        <div class="chart-container">
                            <canvas id="rateChart"></canvas>
                        </div>
                    </div>
                    <div class="content-card p-6 md:p-8">
                        <h3 class="text-2xl font-bold text-[#8D7B68] mb-4">Case 2: Cyclohexyl Halides</h3>
                        <p class="mb-6 text-base md:text-lg leading-relaxed">In cyclohexane rings, reactivity depends on the leaving group's position. An <strong class="font-semibold text-[#65451F]">axial</strong> leaving group allows for an unhindered backside attack. An <strong class="font-semibold text-[#65451F]">equatorial</strong> leaving group forces the nucleophile to attack through the ring, which is sterically blocked. Therefore, axial halides react much faster in Sₙ2 reactions.</p>
                        <div class="grid grid-cols-1 md:grid-cols-2 gap-4 text-center mt-4">
                            <div class="diagram-box p-4 rounded-lg">
                                <h4 class="font-semibold mb-2">Axial Leaving Group (Faster)</h4>
                                <div class="flex items-center justify-center">
                                    <span class="text-blue-500 font-bold">Nu:⁻</span>
                                    <span class="arrow mx-2">&rarr;</span>
                                    <div class="relative">
                                        <span class="font-mono text-xl">C</span>
                                        <span class="absolute top-0 right-0 -mt-4 -mr-2 font-mono text-red-500">X</span>
                                    </div>
                                </div>
                                <p class="text-sm mt-2">Open path for backside attack.</p>
                            </div>
                            <div class="diagram-box p-4 rounded-lg">
                                <h4 class="font-semibold mb-2">Equatorial Leaving Group (Slower)</h4>
                                <div class="flex items-center justify-center">
                                    <span class="text-blue-500 font-bold">Nu:⁻</span>
                                    <span class="arrow mx-2 text-gray-300">&rarr;</span>
                                    <div class="relative">
                                        <span class="font-mono text-xl">C</span>
                                        <span class="absolute top-1/2 right-0 -translate-y-1/2 mr-4 font-mono text-red-500">X</span>
                                    </div>
                                </div>
                                <p class="text-sm mt-2">Attack blocked by ring structure.</p>
                            </div>
                        </div>
                    </div>
                </div>
            </div>

            <div id="competition" class="tab-content hidden">
                 <div class="content-card p-6 md:p-8">
                    <h2 class="text-2xl font-bold text-[#8D7B68] mb-4">Competition with Elimination (E2)</h2>
                    <div class="space-y-4 text-base md:text-lg leading-relaxed">
                        <p>When the Sₙ2 pathway is blocked by severe steric hindrance, a competing reaction, <strong class="font-semibold text-[#65451F]">E2 elimination</strong>, often takes over. Instead of attacking the alpha-carbon, the nucleophile acts as a base and removes a proton from a beta-carbon. This is a less sterically demanding process and leads to the formation of an alkene.</p>
                        <p>This competition is a critical concept in synthesis. For substrates with significant beta-branching (like secondary or the neopentyl-type halides), using a strong, bulky base (like *tert*-butoxide) will almost exclusively favor the E2 product over the Sₙ2 product. The choice between substitution and elimination is therefore a direct consequence of the substrate's steric environment.</p>
                    </div>
                </div>
            </div>

            <div id="gemini-tutor" class="mt-12">
                <div class="content-card p-6 md:p-8">
                    <h2 class="text-2xl font-bold text-[#8D7B68] mb-4">Ask the Chemistry Tutor ✨</h2>
                    <p class="mb-4 text-base md:text-lg text-gray-600">Have a specific question about Sₙ2, E2, or steric hindrance? Our AI tutor can help! Ask a question and get a detailed explanation.</p>
                    <textarea id="questionInput" class="w-full h-32 p-3 text-gray-700 bg-gray-50 border border-gray-300 rounded-lg focus:outline-none focus:border-[#C8B6A6] resize-none" placeholder="E.g., Why is a tertiary alkyl halide unreactive in SN2 reactions?"></textarea>
                    <div class="flex items-center gap-4 mt-4">
                        <button onclick="askGemini()" class="px-6 py-3 bg-[#65451F] text-white rounded-lg font-semibold hover:bg-[#8D7B68] transition-colors duration-200">Ask Gemini ✨</button>
                        <div id="loading" class="hidden loading-spinner"></div>
                    </div>
                    <div id="answerOutput" class="mt-6 p-4 bg-gray-50 border border-gray-200 rounded-lg hidden text-gray-700 whitespace-pre-wrap"></div>
                </div>
            </div>
        </main>
    </div>

    <script>
        const tabs = document.querySelectorAll('.tab');
        const tabContents = document.querySelectorAll('.tab-content');

        function showTab(tabId) {
            tabs.forEach(tab => {
                tab.classList.remove('tab-active');
                tab.classList.add('tab-inactive');
            });
            tabContents.forEach(content => {
                content.classList.add('hidden');
            });

            const activeTab = document.querySelector(`button[onclick="showTab('${tabId}')"]`);
            const activeContent = document.getElementById(tabId);

            activeTab.classList.add('tab-active');
            activeTab.classList.remove('tab-inactive');
            activeContent.classList.remove('hidden');
        }

        const rateData = {
            labels: ['Methyl', 'Ethyl (1°)', 'Isobutyl (1°, β-branched)', 'Isopropyl (2°)', 'Neopentyl (1°, β-quaternary)', 'tert-Butyl (3°)'],
            datasets: [{
                label: 'Relative Sₙ2 Rate',
                data: [30, 1, 0.03, 0.025, 0.00001, 0],
                backgroundColor: [
                    'rgba(141, 123, 104, 0.6)',
                    'rgba(164, 144, 124, 0.6)',
                    'rgba(190, 173, 158, 0.6)',
                    'rgba(200, 182, 166, 0.6)',
                    'rgba(223, 211, 200, 0.6)',
                    'rgba(239, 235, 230, 0.6)'
                ],
                borderColor: [
                    '#8D7B68',
                    '#A4907C',
                    '#BEAD9E',
                    '#C8B6A6',
                    '#DFD3C8',
                    '#EFEBE6'
                ],
                borderWidth: 2
            }]
        };

        const config = {
            type: 'bar',
            data: rateData,
            options: {
                responsive: true,
                maintainAspectRatio: false,
                scales: {
                    y: {
                        type: 'logarithmic',
                        beginAtZero: false,
                        title: {
                            display: true,
                            text: 'Relative Rate (Log Scale)',
                            font: {
                                size: 14,
                                weight: 'bold',
                                family: 'Inter'
                            }
                        },
                        ticks: {
                            color: '#4A4A4A',
                            font: {
                                family: 'Inter'
                            }
                        }
                    },
                    x: {
                       ticks: {
                            color: '#4A4A4A',
                            font: {
                                family: 'Inter'
                            },
                            callback: function(value, index, values) {
                                const label = this.getLabelForValue(value);
                                if (label.length > 16) {
                                    return label.split(' ').map(word => word.length > 8 ? word.substring(0, 8) + '...' : word);
                                }
                                return label;
                            }
                        }
                    }
                },
                plugins: {
                    legend: {
                        display: false
                    },
                    tooltip: {
                        enabled: true,
                        backgroundColor: '#4A4A4A',
                        titleFont: { size: 14, weight: 'bold', family: 'Inter' },
                        bodyFont: { size: 12, family: 'Inter' },
                        callbacks: {
                            label: function(context) {
                                let label = context.dataset.label || '';
                                if (label) {
                                    label += ': ';
                                }
                                if (context.parsed.y !== null) {
                                    if (context.parsed.y < 0.001) {
                                        label += context.parsed.y.toExponential(1);
                                    } else {
                                       label += context.parsed.y;
                                    }
                                }
                                return label;
                            }
                        }
                    }
                }
            }
        };

        async function askGemini() {
            const questionInput = document.getElementById('questionInput');
            const answerOutput = document.getElementById('answerOutput');
            const loadingSpinner = document.getElementById('loading');
            
            const prompt = questionInput.value.trim();
            if (!prompt) return;

            answerOutput.classList.add('hidden');
            loadingSpinner.classList.remove('hidden');

            const fullPrompt = `You are an expert organic chemistry tutor. Please provide a clear, concise, and helpful explanation for the following question about SN2 and E2 reactions:\n\n${prompt}`;

            try {
                let chatHistory = [];
                chatHistory.push({ role: "user", parts: [{ text: fullPrompt }] });
                const payload = { contents: chatHistory };
                const apiKey = "" 
                const apiUrl = `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash-preview-05-20:generateContent?key=${apiKey}`;
                
                const response = await fetch(apiUrl, {
                    method: 'POST',
                    headers: { 'Content-Type': 'application/json' },
                    body: JSON.stringify(payload)
                });
                const result = await response.json();
                
                if (result.candidates && result.candidates.length > 0 &&
                    result.candidates[0].content && result.candidates[0].content.parts &&
                    result.candidates[0].content.parts.length > 0) {
                    const text = result.candidates[0].content.parts[0].text;
                    answerOutput.textContent = text;
                    answerOutput.classList.remove('hidden');
                } else {
                    answerOutput.textContent = "Sorry, I couldn't get a response. Please try again.";
                    answerOutput.classList.remove('hidden');
                }
            } catch (error) {
                console.error("Error fetching from Gemini API:", error);
                answerOutput.textContent = "An error occurred. Please check your network and try again.";
                answerOutput.classList.remove('hidden');
            } finally {
                loadingSpinner.classList.add('hidden');
            }
        }

        window.onload = function() {
            const ctx = document.getElementById('rateChart').getContext('2d');
            new Chart(ctx, config);
        };
    </script>
</body>
</html>
