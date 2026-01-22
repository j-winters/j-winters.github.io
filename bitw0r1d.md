---
layout: raw
permalink: /bitw0r1d/
---
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>bitw0r1d - Winters & Charbonneau (2026)</title>
    <script crossorigin src="https://unpkg.com/react@18/umd/react.production.min.js"></script>
    <script crossorigin src="https://unpkg.com/react-dom@18/umd/react-dom.production.min.js"></script>
    <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=JetBrains+Mono:wght@400;600;700&family=Source+Sans+3:wght@400;600;700&display=swap');
        
        body {
            font-family: 'Source Sans 3', sans-serif;
        }
        
        .mono {
            font-family: 'JetBrains Mono', monospace;
        }
        
        details > summary {
            cursor: pointer;
            list-style: none;
        }
        
        details > summary::-webkit-details-marker {
            display: none;
        }
        
        details > summary::before {
            content: '▶';
            display: inline-block;
            margin-right: 0.5rem;
            transition: transform 0.2s ease;
        }
        
        details[open] > summary::before {
            transform: rotate(90deg);
        }
    </style>
</head>
<body class="bg-slate-900">
    <div id="root"></div>

    <script type="text/babel">
        const { useState, useEffect, useRef } = React;
        
        // Color scheme
        const COLORS = {
            cultural: {
                primary: '#3b82f6', // blue-500
                light: '#dbeafe',   // blue-100
                dark: '#1e40af',    // blue-800
                bg: '#eff6ff'       // blue-50
            },
            search: {
                primary: '#f59e0b', // amber-500
                light: '#fef3c7',   // amber-100
                dark: '#b45309',    // amber-700
                bg: '#fffbeb'       // amber-50
            }
        };
        
        // Simple icon components
        const PlayIcon = () => (
          <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round">
            <polygon points="5 3 19 12 5 21 5 3"></polygon>
          </svg>
        );
        
        const PauseIcon = () => (
          <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round">
            <rect x="6" y="4" width="4" height="16"></rect>
            <rect x="14" y="4" width="4" height="16"></rect>
          </svg>
        );
        
        const RotateCcwIcon = () => (
          <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round">
            <polyline points="1 4 1 10 7 10"></polyline>
            <path d="M3.51 15a9 9 0 1 0 2.13-9.36L1 10"></path>
          </svg>
        );
        
        const DownloadIcon = () => (
          <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round">
            <path d="M21 15v4a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2v-4"></path>
            <polyline points="7 10 12 15 17 10"></polyline>
            <line x1="12" y1="15" x2="12" y2="3"></line>
          </svg>
        );
        
        const SettingsIcon = () => (
          <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round">
            <circle cx="12" cy="12" r="3"></circle>
            <path d="M19.4 15a1.65 1.65 0 0 0 .33 1.82l.06.06a2 2 0 0 1 0 2.83 2 2 0 0 1-2.83 0l-.06-.06a1.65 1.65 0 0 0-1.82-.33 1.65 1.65 0 0 0-1 1.51V21a2 2 0 0 1-2 2 2 2 0 0 1-2-2v-.09A1.65 1.65 0 0 0 9 19.4a1.65 1.65 0 0 0-1.82.33l-.06.06a2 2 0 0 1-2.83 0 2 2 0 0 1 0-2.83l.06-.06a1.65 1.65 0 0 0 .33-1.82 1.65 1.65 0 0 0-1.51-1H3a2 2 0 0 1-2-2 2 2 0 0 1 2-2h.09A1.65 1.65 0 0 0 4.6 9a1.65 1.65 0 0 0-.33-1.82l-.06-.06a2 2 0 0 1 0-2.83 2 2 0 0 1 2.83 0l.06.06a1.65 1.65 0 0 0 1.82.33H9a1.65 1.65 0 0 0 1-1.51V3a2 2 0 0 1 2-2 2 2 0 0 1 2 2v.09a1.65 1.65 0 0 0 1 1.51 1.65 1.65 0 0 0 1.82-.33l.06-.06a2 2 0 0 1 2.83 0 2 2 0 0 1 0 2.83l-.06.06a1.65 1.65 0 0 0-.33 1.82V9a1.65 1.65 0 0 0 1.51 1H21a2 2 0 0 1 2 2 2 2 0 0 1-2 2h-.09a1.65 1.65 0 0 0-1.51 1z"></path>
          </svg>
        );

        // Collapsible section component
        const CollapsibleSection = ({ title, children, defaultOpen = false }) => (
          <details open={defaultOpen} className="mb-4">
            <summary className="text-base font-semibold text-slate-300 hover:text-white transition-colors py-2">
              {title}
            </summary>
            <div className="text-base text-slate-400 mt-2 pl-5 space-y-2 leading-relaxed">
              {children}
            </div>
          </details>
        );

        const BitstringEvolution = () => {
          const [culturalSystem, setCulturalSystem] = useState('10110101');
          const [searchSpace, setSearchSpace] = useState('11010110');
          const [isRunning, setIsRunning] = useState(false);
          const [eta, setEta] = useState(0.99);
          const [lambda, setLambda] = useState(0.05);
          const [pTradeoff, setPTradeoff] = useState(0.5);
          const [initialEndowment, setInitialEndowment] = useState(100);
          const [resourceStore, setResourceStore] = useState(100);
          const [isDepleted, setIsDepleted] = useState(false);
          const [generation, setGeneration] = useState(0);
          const [history, setHistory] = useState([]);
          const [speed, setSpeed] = useState(100); // Changed default to 100ms
          const [seed, setSeed] = useState(12345);
          const [initialCulturalComplexity, setInitialCulturalComplexity] = useState(8);
          const [initialSearchComplexity, setInitialSearchComplexity] = useState(8);
          const [initialTech, setInitialTech] = useState('');
          const [initialSpace, setInitialSpace] = useState('');
          const intervalRef = useRef(null);
          
          // Chart settings
          const [complexityXScale, setComplexityXScale] = useState('linear');
          const [complexityYScale, setComplexityYScale] = useState('linear');
          const [useFixedAxes, setUseFixedAxes] = useState(false);
          const [fixedXMax, setFixedXMax] = useState(10000);
          const [fixedYMax, setFixedYMax] = useState(10000);
          const [showChartSettings, setShowChartSettings] = useState(false);

          // Initialize on mount
          useEffect(() => {
            const newTech = generateBitstring(initialCulturalComplexity, seed);
            const newSpace = generateBitstring(initialSearchComplexity, seed + 1);
            setCulturalSystem(newTech);
            setSearchSpace(newSpace);
            setInitialTech(newTech);
            setInitialSpace(newSpace);
          }, []);

          // Seeded random number generator (simple LCG)
          const seededRandom = (seed) => {
            let state = seed;
            return () => {
              state = (state * 1664525 + 1013904223) % 4294967296;
              return state / 4294967296;
            };
          };

          // Generate random bitstring with seed
          const generateBitstring = (length, seedValue) => {
            const rng = seededRandom(seedValue);
            const prob = rng();
            let result = '';
            for (let i = 0; i < length; i++) {
              result += rng() < prob ? '1' : '0';
            }
            return result;
          };

          // Calculate normalized inverted Levenshtein distance (effectiveness)
          const calculateEffectiveness = (tech, space) => {
            const maxLen = Math.max(tech.length, space.length);
            if (maxLen === 0) return 1;
            
            const levenshtein = (s1, s2) => {
              const matrix = [];
              for (let i = 0; i <= s2.length; i++) {
                matrix[i] = [i];
              }
              for (let j = 0; j <= s1.length; j++) {
                matrix[0][j] = j;
              }
              for (let i = 1; i <= s2.length; i++) {
                for (let j = 1; j <= s1.length; j++) {
                  if (s2.charAt(i - 1) === s1.charAt(j - 1)) {
                    matrix[i][j] = matrix[i - 1][j - 1];
                  } else {
                    matrix[i][j] = Math.min(
                      matrix[i - 1][j - 1] + 1,
                      matrix[i][j - 1] + 1,
                      matrix[i - 1][j] + 1
                    );
                  }
                }
              }
              return matrix[s2.length][s1.length];
            };
            
            const distance = levenshtein(tech, space);
            return 1 - (distance / maxLen);
          };

          // Calculate resources (matching Python resource_function)
          const calculateResources = (lengthSpace, effectiveness, lengthTech) => {
            return (lengthSpace * effectiveness) - (lengthTech * (1 - effectiveness));
          };

          // Update resource store (matching Python update_resource_store)
          const updateResourceStore = (netResources, currentStore) => {
            if (netResources >= 1) {
              return { availableResources: netResources, newStore: currentStore, depleted: false };
            } else {
              let deficit;
              if (netResources >= 0) {
                deficit = 1;
              } else {
                deficit = Math.abs(netResources);
              }
              
              if (currentStore >= deficit) {
                return { 
                  availableResources: 1.0, 
                  newStore: currentStore - deficit, 
                  depleted: false 
                };
              } else {
                return { 
                  availableResources: currentStore, 
                  newStore: 0, 
                  depleted: true 
                };
              }
            }
          };

          // Apply mutation: insertion, deletion, or substitution
          const mutate = (bitstring) => {
            if (bitstring.length === 0) {
              return Math.random() < 0.5 ? '0' : '1';
            }
            
            const mutationType = Math.random();
            
            if (mutationType < 1/3) {
              // Substitution (flip)
              const position = Math.floor(Math.random() * bitstring.length);
              const newBit = bitstring[position] === '0' ? '1' : '0';
              return bitstring.slice(0, position) + newBit + bitstring.slice(position + 1);
            } else if (mutationType < 2/3) {
              // Insertion
              const position = Math.floor(Math.random() * (bitstring.length + 1));
              const newBit = Math.random() < 0.5 ? '0' : '1';
              return bitstring.slice(0, position) + newBit + bitstring.slice(position);
            } else {
              // Deletion
              if (bitstring.length <= 1) return bitstring;
              const position = Math.floor(Math.random() * bitstring.length);
              return bitstring.slice(0, position) + bitstring.slice(position + 1);
            }
          };

          // Cultural evolution of technologies (matching ce_technologies)
          const evolveTechnologies = (tech, space, etaParam) => {
            const candidate = mutate(tech);
            const currentE = calculateEffectiveness(tech, space);
            const candidateE = calculateEffectiveness(candidate, space);
            
            if (Math.random() < etaParam) {
              if (candidateE > currentE) {
                return candidate;
              }
              return tech;
            } else {
              return candidate;
            }
          };

          // Cultural evolution of search spaces (matching ce_space)
          const evolveSearchSpace = (space, tech, lambdaParam) => {
            const candidate = mutate(space);
            const currentE = calculateEffectiveness(tech, space);
            const candidateE = calculateEffectiveness(tech, candidate);
            
            if (Math.random() < lambdaParam) {
              if (candidateE > currentE) {
                return candidate;
              }
              return space;
            } else {
              return candidate;
            }
          };

          // Evolution step (matching the main simulation loop)
          const evolve = () => {
            if (isDepleted) {
              setIsRunning(false);
              return;
            }

            const currentEffectiveness = calculateEffectiveness(culturalSystem, searchSpace);
            const netResources = calculateResources(
              searchSpace.length,
              currentEffectiveness,
              culturalSystem.length
            );
            
            const { availableResources, newStore, depleted } = updateResourceStore(netResources, resourceStore);
            
            if (depleted) {
              setIsDepleted(true);
              setResourceStore(newStore);
              setIsRunning(false);
              return;
            }
            
            const iterations = availableResources >= 1 ? Math.round(availableResources) : 1;
            
            let newCC = culturalSystem;
            let newSS = searchSpace;
            
            if (iterations > 0) {
              for (let i = 0; i < iterations; i++) {
                if (Math.random() < pTradeoff) {
                  newCC = evolveTechnologies(newCC, newSS, eta);
                } else {
                  newSS = evolveSearchSpace(newSS, newCC, lambda);
                }
              }
            }
            
            const newEffectiveness = calculateEffectiveness(newCC, newSS);
            const newNetResources = calculateResources(newSS.length, newEffectiveness, newCC.length);
            
            setHistory(prev => [...prev, {
              generation: generation + 1,
              cc: newCC,
              ss: newSS,
              effectiveness: newEffectiveness,
              netResources: newNetResources,
              availableResources: availableResources,
              resourceStore: newStore,
              lengthCC: newCC.length,
              lengthSS: newSS.length
            }]);
            
            setCulturalSystem(newCC);
            setSearchSpace(newSS);
            setResourceStore(newStore);
            setGeneration(prev => prev + 1);
          };

          useEffect(() => {
            if (isRunning) {
              intervalRef.current = setInterval(evolve, speed);
            } else {
              if (intervalRef.current) {
                clearInterval(intervalRef.current);
              }
            }
            return () => {
              if (intervalRef.current) {
                clearInterval(intervalRef.current);
              }
            };
          }, [isRunning, culturalSystem, searchSpace, eta, lambda, pTradeoff, resourceStore, isDepleted, generation, speed]);

          const reset = () => {
            setIsRunning(false);
            const newTech = generateBitstring(initialCulturalComplexity, seed);
            const newSpace = generateBitstring(initialSearchComplexity, seed + 1);
            setCulturalSystem(newTech);
            setSearchSpace(newSpace);
            setInitialTech(newTech);
            setInitialSpace(newSpace);
            setResourceStore(initialEndowment);
            setIsDepleted(false);
            setGeneration(0);
            setHistory([]);
          };

          // CSV download function
          const downloadCSV = () => {
            if (history.length === 0) return;
            
            const headers = [
              'seed',
              'generation',
              'culture_complexity',
              'space_complexity',
              'effectiveness',
              'initial_tech',
              'initial_space',
              'eta',
              'lambda',
              'initial_endowment',
              'p_tradeoff',
              'available_resources',
              'resource_store'
            ];
            
            const rows = history.map(h => [
              seed,
              h.generation,
              h.lengthCC,
              h.lengthSS,
              h.effectiveness.toFixed(6),
              initialTech,
              initialSpace,
              eta.toFixed(2),
              lambda.toFixed(2),
              initialEndowment,
              pTradeoff.toFixed(2),
              h.availableResources.toFixed(6),
              h.resourceStore.toFixed(6)
            ]);
            
            const csvContent = [
              headers.join(','),
              ...rows.map(row => row.join(','))
            ].join('\n');
            
            const blob = new Blob([csvContent], { type: 'text/csv;charset=utf-8;' });
            const link = document.createElement('a');
            const url = URL.createObjectURL(blob);
            link.setAttribute('href', url);
            link.setAttribute('download', `bitw0r1d_seed${seed}_gen${generation}.csv`);
            link.style.visibility = 'hidden';
            document.body.appendChild(link);
            link.click();
            document.body.removeChild(link);
          };

          const effectiveness = calculateEffectiveness(culturalSystem, searchSpace);
          const netResources = calculateResources(searchSpace.length, effectiveness, culturalSystem.length);
          const { availableResources } = updateResourceStore(netResources, resourceStore);

          // Scale transformation functions for charts
          const transformValue = (value, scale, maxValue) => {
            if (scale === 'log') {
              if (value <= 0) return 0;
              const logMax = Math.log10(maxValue);
              const logVal = Math.log10(value);
              return Math.max(0, logVal / logMax);
            }
            return value / maxValue;
          };

          return (
            <div className="w-full max-w-6xl mx-auto p-6 bg-gradient-to-br from-slate-800 to-slate-900 rounded-lg shadow-2xl">
              <h1 className="text-3xl font-bold mb-2 text-white">
                bitw<span className="text-blue-400">0</span>r<span className="text-amber-400">1</span>d: A co-evolutionary model of cultural systems and search spaces
              </h1>
              
              <div className="text-slate-300 mb-6 text-base">
                <CollapsibleSection title="About this simulation" defaultOpen={false}>
                  <p className="text-base leading-relaxed">
                    This is an online simulation of the bitw0r1d model presented in <a href="https://arxiv.org/abs/2508.04828" className="text-blue-400 hover:text-blue-300 underline">Winters & Charbonneau (2026)</a>.
                    In bitw0r1d, we model the co-evolutionary relationship between <strong className="text-blue-300">cultural systems</strong> (the repertoire of interacting techniques, artifacts, norms and knowledge available to a society)
                    and <strong className="text-amber-400">search spaces</strong> (the collection of needs, problems and goals faced by societies at a given point in time).
                    For our paper, we were interested in how cultural systems and search spaces co-evolved with one another, especially with respect to the emergence of open-ended dynamics.
                  </p>
                </CollapsibleSection>
                
                <CollapsibleSection title="How it works" defaultOpen={false}>
                  <p className="text-base leading-relaxed">
                    Both cultural systems and search spaces are represented as bitstrings that can change via modifications (flipping a randomly selected bit to its opposite value), expansions (inserting a new randomly chosen bit), and 
                    simplifications (where a randomly chosen bit is deleted). Changes can either increase or decrease the match between cultural systems and search spaces (measured as <em>effectiveness</em>).
                    Critically, societies need to produce resources in order to fund evolutionary changes to both cultural systems and search spaces.
                    There are two ways to increase resources here: (i) by improving the match (or fit) between cultural systems or search spaces, or (ii) by expanding the search space (which serves as an upper-bound on the maximum obtainable resources).
                    Societies start with an initial resource buffer and will collapse if resources run out. The simulation allows you to investigate whether societies can sustain co-evolution and achieve open-ended dynamics.
                  </p>
                </CollapsibleSection>
                
                <CollapsibleSection title="Key parameters" defaultOpen={false}>
                  <p className="text-base leading-relaxed">
                    <em className="text-blue-300">η (eta)</em> controls how much selection pressure acts on cultural systems 
                    (0 = purely random changes, 1 = only accept improvements that increase the fit with the current search space). 
                    <em className="text-amber-300"> λ (lambda)</em> does the same for search spaces (0 = purely random changes, 1 = only accept improvements that increase the fit with the existing cultural system). 
                    The <em className="text-purple-300">tradeoff (p)</em> determines how resources are split between changing cultural systems versus search spaces. 
                    You can also set the initial complexity of cultural systems and search spaces as well as change the random seed before running.
                  </p>
                </CollapsibleSection>
                
                <CollapsibleSection title="Measures" defaultOpen={false}>
                  <p className="text-base leading-relaxed">
                    Once the simulation is running, you will see both bitstrings for cultural systems and search spaces as they change over time.
                    You will also see values for <em className="text-purple-300">Effectiveness</em> (the normalised distance between cultural systems and search spaces, ranging from 0 to 1 where 1 is a perfect match), <em className="text-emerald-300">Complexity</em> (the length of the bitstrings, where longer strings denote more complex cultural systems and search spaces),
                    <em className="text-green-300"> Available Resources</em> (the resources a society actually has to work with each generation), and the <em className="text-blue-300">Resource Store</em> (set at 100 by default; backup resources to use if the available resources drops below 1).
                  </p>
                </CollapsibleSection>
                
                <CollapsibleSection title="Things to try" defaultOpen={false}>
                  <p className="text-base leading-relaxed">
                    Try setting the initial complexity for cultural systems and search spaces to ℓ=2. 
                    Then, play around with different parameter values for η and λ as we did in the paper. This will help you develop an intuitive understanding of the model.
                    Importantly, remember to try out the same parameter values with <em>different</em> random seeds. See if you can find parameter values and seeds that lead to open-ended dynamics... 
                  </p>
                </CollapsibleSection>
                
                <CollapsibleSection title="Bonus features" defaultOpen={false}>
                  <p className="text-base leading-relaxed">
                    A unique feature of this online version is that you can adjust selection pressures during a run — try starting with high stochasticity to build initial complexity, then gradually increasing selection strength to see if you can stabilise and sustain growth. 
                    This real-time manipulation wasn't possible in the Python model, so please feel free to <a href="mailto:james.winters@brunel.ac.uk" className="text-blue-400 hover:text-blue-300 underline">send an email</a> if you find anything interesting!
                  </p>
                </CollapsibleSection>
              </div>
              
              <div className="bg-slate-700/50 p-6 rounded-lg shadow mb-6 border border-slate-600">
                <div className="flex gap-4 mb-6 flex-wrap">
                  <button
                    onClick={() => setIsRunning(!isRunning)}
                    disabled={isDepleted}
                    className={`flex items-center gap-2 px-6 py-3 rounded-lg transition-all font-semibold shadow-lg ${
                      isDepleted 
                        ? 'bg-gray-600 text-gray-400 cursor-not-allowed' 
                        : 'bg-blue-600 text-white hover:bg-blue-500 hover:shadow-blue-500/25'
                    }`}
                  >
                    {isRunning ? <PauseIcon /> : <PlayIcon />}
                    {isRunning ? 'Pause' : isDepleted ? 'Depleted' : 'Run'}
                  </button>
                  <button
                    onClick={reset}
                    className="flex items-center gap-2 px-6 py-3 bg-slate-600 text-white rounded-lg hover:bg-slate-500 transition-all font-semibold shadow-lg"
                  >
                    <RotateCcwIcon />
                    {generation === 0 ? 'Initialize' : 'Reset'}
                  </button>
                  <button
                    onClick={downloadCSV}
                    disabled={history.length === 0}
                    className={`flex items-center gap-2 px-6 py-3 rounded-lg transition-all font-semibold shadow-lg ${
                      history.length === 0
                        ? 'bg-gray-600 text-gray-400 cursor-not-allowed'
                        : 'bg-amber-600 text-white hover:bg-amber-500 hover:shadow-amber-500/25'
                    }`}
                  >
                    <DownloadIcon />
                    Download CSV
                  </button>
                </div>

                <div className="mb-6 p-4 bg-slate-800/50 rounded-lg border border-slate-600">
                  <h3 className="text-sm font-bold text-slate-300 mb-3">Initial Setup (reset to apply)</h3>
                  <div className="grid grid-cols-1 md:grid-cols-3 gap-4">
                    <div>
                      <label className="block text-sm font-semibold text-slate-400 mb-2">
                        Random Seed: {seed}
                      </label>
                      <input
                        type="number"
                        value={seed}
                        onChange={(e) => setSeed(parseInt(e.target.value) || 0)}
                        disabled={generation > 0}
                        className="w-full px-3 py-2 border border-slate-500 rounded bg-slate-700 text-white disabled:opacity-50"
                      />
                    </div>

                    <div>
                      <label className="block text-sm font-semibold text-slate-400 mb-2">
                        Initial Cultural System Complexity: {initialCulturalComplexity}
                      </label>
                      <input
                        type="range"
                        min="1"
                        max="20"
                        step="1"
                        value={initialCulturalComplexity}
                        onChange={(e) => setInitialCulturalComplexity(parseInt(e.target.value))}
                        disabled={generation > 0}
                        className="w-full accent-blue-500"
                      />
                    </div>

                    <div>
                      <label className="block text-sm font-semibold text-slate-400 mb-2">
                        Initial Search Space Complexity: {initialSearchComplexity}
                      </label>
                      <input
                        type="range"
                        min="1"
                        max="20"
                        step="1"
                        value={initialSearchComplexity}
                        onChange={(e) => setInitialSearchComplexity(parseInt(e.target.value))}
                        disabled={generation > 0}
                        className="w-full accent-amber-500"
                      />
                    </div>
                  </div>
                  {generation > 0 && (
                    <p className="text-xs text-amber-400 mt-2">Reset simulation to change initial parameters</p>
                  )}
                </div>

                <div className="grid grid-cols-1 md:grid-cols-2 gap-6">
                  <div>
                    <label className="block text-sm font-semibold text-slate-400 mb-2">
                      η (Eta - Cultural System Selection): {eta.toFixed(2)}
                    </label>
                    <input
                      type="range"
                      min="0"
                      max="1"
                      step="0.01"
                      value={eta}
                      onChange={(e) => setEta(parseFloat(e.target.value))}
                      className="w-full accent-blue-500"
                    />
                    <p className="text-xs text-slate-500 mt-1">0 = random drift, 1 = strong selection on effectiveness</p>
                  </div>

                  <div>
                    <label className="block text-sm font-semibold text-slate-400 mb-2">
                      λ (Lambda - Space Selection): {lambda.toFixed(2)}
                    </label>
                    <input
                      type="range"
                      min="0"
                      max="1"
                      step="0.01"
                      value={lambda}
                      onChange={(e) => setLambda(parseFloat(e.target.value))}
                      className="w-full accent-amber-500"
                    />
                    <p className="text-xs text-slate-500 mt-1">0 = random drift, 1 = strong selection on effectiveness</p>
                  </div>

                  <div>
                    <label className="block text-sm font-semibold text-slate-400 mb-2">
                      p (Tradeoff): {pTradeoff.toFixed(2)}
                    </label>
                    <input
                      type="range"
                      min="0"
                      max="1"
                      step="0.01"
                      value={pTradeoff}
                      onChange={(e) => setPTradeoff(parseFloat(e.target.value))}
                      className="w-full accent-purple-500"
                    />
                    <p className="text-xs text-slate-500 mt-1">Probability of allocating resources to cultural systems (vs search spaces)</p>
                  </div>

                  <div>
                    <label className="block text-sm font-semibold text-slate-400 mb-2">
                      Initial Endowment: {initialEndowment}
                    </label>
                    <input
                      type="range"
                      min="10"
                      max="500"
                      step="10"
                      value={initialEndowment}
                      onChange={(e) => {
                        const val = parseInt(e.target.value);
                        setInitialEndowment(val);
                        setResourceStore(val);
                      }}
                      disabled={generation > 0}
                      className="w-full accent-emerald-500"
                    />
                    <p className="text-xs text-slate-500 mt-1">Starting resource store (reset to change)</p>
                  </div>

                  <div>
                    <label className="block text-sm font-semibold text-slate-400 mb-2">
                      Speed (ms): {speed}
                    </label>
                    <input
                      type="range"
                      min="10"
                      max="2000"
                      step="10"
                      value={speed}
                      onChange={(e) => setSpeed(parseInt(e.target.value))}
                      className="w-full accent-slate-400"
                    />
                  </div>
                </div>

                {isDepleted && (
                  <div className="mt-4 p-4 bg-red-900/50 border-l-4 border-red-500 text-red-200 rounded">
                    <strong>Resources Depleted!</strong> The society has collapsed at generation {generation}. Reset to continue.
                  </div>
                )}
              </div>

              <div className="bg-slate-700/50 p-6 rounded-lg shadow mb-6 border border-slate-600">
                <h2 className="text-xl font-bold mb-4 text-white">Generation {generation}</h2>
                
                <div className="space-y-4">
                  <div className="border-l-4 border-blue-500 pl-4">
                    <div className="text-sm font-semibold text-slate-400 mb-1">Cultural System (ℓ={culturalSystem.length})</div>
                    <div className="mono text-lg tracking-wider break-all bg-blue-950/50 p-3 rounded border border-blue-800">
                      {culturalSystem.split('').map((bit, idx) => (
                        <span key={idx} className={bit === '1' ? 'text-blue-400 font-bold' : 'text-slate-500'}>
                          {bit}
                        </span>
                      ))}
                    </div>
                  </div>

                  <div className="border-l-4 border-amber-500 pl-4">
                    <div className="text-sm font-semibold text-slate-400 mb-1">Search Space (ℓ={searchSpace.length})</div>
                    <div className="mono text-lg tracking-wider break-all bg-amber-950/50 p-3 rounded border border-amber-800">
                      {searchSpace.split('').map((bit, idx) => (
                        <span key={idx} className={bit === '1' ? 'text-amber-400 font-bold' : 'text-slate-500'}>
                          {bit}
                        </span>
                      ))}
                    </div>
                  </div>
                </div>

                <div className="grid grid-cols-3 gap-4 mt-6">
                  <div className="bg-gradient-to-br from-purple-900/50 to-purple-800/30 p-4 rounded-lg border border-purple-700">
                    <div className="text-sm font-semibold text-slate-400">Effectiveness (E)</div>
                    <div className="text-2xl font-bold text-purple-300">{effectiveness.toFixed(3)}</div>
                    <div className="text-xs text-slate-500 mt-1">The distance between cultural systems and search spaces</div>
                  </div>
                  <div className="bg-gradient-to-br from-emerald-900/50 to-emerald-800/30 p-4 rounded-lg border border-emerald-700">
                    <div className="text-sm font-semibold text-slate-400">Available Resources</div>
                    <div className="text-2xl font-bold text-emerald-300">{availableResources.toFixed(2)}</div>
                    <div className="text-xs text-slate-500 mt-1">{Math.round(availableResources)} iterations this generation</div>
                  </div>
                  <div className="bg-gradient-to-br from-blue-900/50 to-blue-800/30 p-4 rounded-lg border border-blue-700">
                    <div className="text-sm font-semibold text-slate-400">Resource Store</div>
                    <div className="text-2xl font-bold text-blue-300">{resourceStore.toFixed(2)}</div>
                    <div className="text-xs text-slate-500 mt-1">Reserve buffer capacity</div>
                  </div>
                </div>
              </div>

              {history.length > 0 && (
                <div className="bg-slate-700/50 p-6 rounded-lg shadow border border-slate-600">
                  <div className="flex justify-between items-center mb-4">
                    <h2 className="text-xl font-bold text-white">Evolution History</h2>
                  </div>
                  
                  <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6 mb-4">
                    <div>
                      <div className="text-sm font-semibold text-slate-400 mb-2">Effectiveness</div>
                      <div className="relative h-40 border border-slate-600 rounded bg-slate-800 p-2">
                        <svg className="w-full h-full" viewBox="0 0 100 100" preserveAspectRatio="none">
                          <line x1="0" y1="100" x2="100" y2="100" stroke="#475569" strokeWidth="0.5" vectorEffect="non-scaling-stroke" />
                          <line x1="0" y1="50" x2="100" y2="50" stroke="#475569" strokeWidth="0.5" vectorEffect="non-scaling-stroke" strokeDasharray="2" />
                          <line x1="0" y1="0" x2="100" y2="0" stroke="#475569" strokeWidth="0.5" vectorEffect="non-scaling-stroke" />
                          {history.map((h, i) => {
                            if (i === 0) return null;
                            const x1 = ((i - 1) / (history.length - 1)) * 100;
                            const x2 = (i / (history.length - 1)) * 100;
                            const y1 = (1 - history[i - 1].effectiveness) * 100;
                            const y2 = (1 - h.effectiveness) * 100;
                            return React.createElement('line', {
                              key: i,
                              x1: x1,
                              y1: y1,
                              x2: x2,
                              y2: y2,
                              stroke: "#a855f7",
                              strokeWidth: "2",
                              vectorEffect: "non-scaling-stroke"
                            });
                          })}
                        </svg>
                        <div className="absolute left-0 top-0 text-xs text-slate-500">1.0</div>
                        <div className="absolute left-0 top-1/2 -mt-2 text-xs text-slate-500">0.5</div>
                        <div className="absolute left-0 bottom-0 text-xs text-slate-500">0.0</div>
                        <div className="absolute bottom-0 left-1/2 -ml-8 text-xs text-slate-500">Generation</div>
                      </div>
                    </div>

                    <div>
                      <div className="flex justify-between items-center mb-2">
                        <div className="text-sm font-semibold text-slate-400">Complexity</div>
                        <button
                          onClick={() => setShowChartSettings(!showChartSettings)}
                          className="text-slate-400 hover:text-white transition-colors p-1 rounded hover:bg-slate-600"
                          title="Chart settings"
                        >
                          <SettingsIcon />
                        </button>
                      </div>
                      
                      {showChartSettings && (
                        <div className="absolute z-10 bg-slate-800 border border-slate-600 rounded-lg p-4 shadow-xl mt-1 w-64">
                          <h4 className="text-sm font-semibold text-white mb-3">Chart Settings</h4>
                          
                          <div className="space-y-3">
                            <div>
                              <label className="block text-xs text-slate-400 mb-1">X-Axis Scale</label>
                              <select
                                value={complexityXScale}
                                onChange={(e) => setComplexityXScale(e.target.value)}
                                className="w-full px-2 py-1 bg-slate-700 border border-slate-600 rounded text-white text-sm"
                              >
                                <option value="linear">Linear</option>
                                <option value="log">Logarithmic</option>
                              </select>
                            </div>
                            
                            <div>
                              <label className="block text-xs text-slate-400 mb-1">Y-Axis Scale</label>
                              <select
                                value={complexityYScale}
                                onChange={(e) => setComplexityYScale(e.target.value)}
                                className="w-full px-2 py-1 bg-slate-700 border border-slate-600 rounded text-white text-sm"
                              >
                                <option value="linear">Linear</option>
                                <option value="log">Logarithmic</option>
                              </select>
                            </div>
                            
                            <div className="flex items-center gap-2">
                              <input
                                type="checkbox"
                                id="fixedAxes"
                                checked={useFixedAxes}
                                onChange={(e) => setUseFixedAxes(e.target.checked)}
                                className="accent-blue-500"
                              />
                              <label htmlFor="fixedAxes" className="text-xs text-slate-400">Use fixed axes</label>
                            </div>
                            
                            {useFixedAxes && (
                              <>
                                <div>
                                  <label className="block text-xs text-slate-400 mb-1">Max X (Generations): {fixedXMax.toLocaleString()}</label>
                                  <input
                                    type="range"
                                    min="100"
                                    max="100000"
                                    step="100"
                                    value={fixedXMax}
                                    onChange={(e) => setFixedXMax(parseInt(e.target.value))}
                                    className="w-full accent-blue-500"
                                  />
                                </div>
                                
                                <div>
                                  <label className="block text-xs text-slate-400 mb-1">Max Y (Complexity): {fixedYMax.toLocaleString()}</label>
                                  <input
                                    type="range"
                                    min="100"
                                    max="100000"
                                    step="100"
                                    value={fixedYMax}
                                    onChange={(e) => setFixedYMax(parseInt(e.target.value))}
                                    className="w-full accent-blue-500"
                                  />
                                </div>
                              </>
                            )}
                          </div>
                          
                          <button
                            onClick={() => setShowChartSettings(false)}
                            className="mt-3 w-full px-3 py-1 bg-slate-600 hover:bg-slate-500 rounded text-white text-sm transition-colors"
                          >
                            Close
                          </button>
                        </div>
                      )}
                      
                      <div className="relative h-40 border border-slate-600 rounded bg-slate-800 p-2">
                        <svg className="w-full h-full" viewBox="0 0 100 100" preserveAspectRatio="none">
                          <line x1="0" y1="100" x2="100" y2="100" stroke="#475569" strokeWidth="0.5" vectorEffect="non-scaling-stroke" />
                          <line x1="0" y1="50" x2="100" y2="50" stroke="#475569" strokeWidth="0.5" vectorEffect="non-scaling-stroke" strokeDasharray="2" />
                          <line x1="0" y1="0" x2="100" y2="0" stroke="#475569" strokeWidth="0.5" vectorEffect="non-scaling-stroke" />
                          {history.map((h, i) => {
                            if (i === 0) return null;
                            
                            const maxGen = useFixedAxes ? fixedXMax : history.length;
                            const maxLen = useFixedAxes ? fixedYMax : Math.max(...history.map(h => Math.max(h.lengthCC, h.lengthSS)));
                            
                            let x1, x2, y1CC, y2CC, y1SS, y2SS;
                            
                            if (complexityXScale === 'log') {
                              x1 = transformValue(i, 'log', maxGen) * 100;
                              x2 = transformValue(i + 1, 'log', maxGen) * 100;
                            } else {
                              x1 = ((i - 1) / (maxGen - 1)) * 100;
                              x2 = (i / (maxGen - 1)) * 100;
                            }
                            
                            if (complexityYScale === 'log') {
                              y1CC = (1 - transformValue(history[i - 1].lengthCC, 'log', maxLen)) * 100;
                              y2CC = (1 - transformValue(h.lengthCC, 'log', maxLen)) * 100;
                              y1SS = (1 - transformValue(history[i - 1].lengthSS, 'log', maxLen)) * 100;
                              y2SS = (1 - transformValue(h.lengthSS, 'log', maxLen)) * 100;
                            } else {
                              y1CC = (1 - history[i - 1].lengthCC / maxLen) * 100;
                              y2CC = (1 - h.lengthCC / maxLen) * 100;
                              y1SS = (1 - history[i - 1].lengthSS / maxLen) * 100;
                              y2SS = (1 - h.lengthSS / maxLen) * 100;
                            }
                            
                            return React.createElement('g', { key: i },
                              React.createElement('line', {
                                x1: x1,
                                y1: y1CC,
                                x2: x2,
                                y2: y2CC,
                                stroke: "#3b82f6",
                                strokeWidth: "2",
                                vectorEffect: "non-scaling-stroke"
                              }),
                              React.createElement('line', {
                                x1: x1,
                                y1: y1SS,
                                x2: x2,
                                y2: y2SS,
                                stroke: "#f59e0b",
                                strokeWidth: "2",
                                vectorEffect: "non-scaling-stroke"
                              })
                            );
                          })}
                        </svg>
                        <div className="absolute left-0 top-0 text-xs text-slate-500">
                          {useFixedAxes ? fixedYMax.toLocaleString() : Math.max(...history.map(h => Math.max(h.lengthCC, h.lengthSS)))}
                        </div>
                        <div className="absolute left-0 bottom-0 text-xs text-slate-500">{complexityYScale === 'log' ? '1' : '0'}</div>
                        <div className="absolute bottom-0 left-1/2 -ml-8 text-xs text-slate-500">Generation</div>
                      </div>
                      <div className="flex gap-4 mt-1 text-xs">
                        <span className="text-blue-400">■ Cultural</span>
                        <span className="text-amber-400">■ Search</span>
                        {(complexityXScale !== 'linear' || complexityYScale !== 'linear') && (
                          <span className="text-slate-500 ml-auto">
                            {complexityXScale === 'log' && 'X: log'} {complexityYScale === 'log' && 'Y: log'}
                          </span>
                        )}
                      </div>
                    </div>

                    <div>
                      <div className="text-sm font-semibold text-slate-400 mb-2">Net Resources</div>
                      <div className="relative h-40 border border-slate-600 rounded bg-slate-800 p-2">
                        <svg className="w-full h-full" viewBox="0 0 100 100" preserveAspectRatio="none">
                          <line x1="0" y1="50" x2="100" y2="50" stroke="#475569" strokeWidth="0.5" vectorEffect="non-scaling-stroke" strokeDasharray="2" />
                          <line x1="0" y1="100" x2="100" y2="100" stroke="#475569" strokeWidth="0.5" vectorEffect="non-scaling-stroke" />
                          <line x1="0" y1="0" x2="100" y2="0" stroke="#475569" strokeWidth="0.5" vectorEffect="non-scaling-stroke" />
                          {history.map((h, i) => {
                            if (i === 0) return null;
                            const maxAbs = Math.max(...history.map(h => Math.abs(h.netResources)), 1);
                            const x1 = ((i - 1) / (history.length - 1)) * 100;
                            const x2 = (i / (history.length - 1)) * 100;
                            const y1 = 50 - (history[i - 1].netResources / maxAbs) * 45;
                            const y2 = 50 - (h.netResources / maxAbs) * 45;
                            return React.createElement('line', {
                              key: i,
                              x1: x1,
                              y1: y1,
                              x2: x2,
                              y2: y2,
                              stroke: h.netResources >= 0 ? "#22c55e" : "#ef4444",
                              strokeWidth: "2",
                              vectorEffect: "non-scaling-stroke"
                            });
                          })}
                        </svg>
                        <div className="absolute left-0 top-0 text-xs text-slate-500">{Math.max(...history.map(h => h.netResources)).toFixed(0)}</div>
                        <div className="absolute left-0 top-1/2 -mt-2 text-xs text-slate-500">0</div>
                        <div className="absolute left-0 bottom-0 text-xs text-slate-500">{Math.min(...history.map(h => h.netResources)).toFixed(0)}</div>
                        <div className="absolute bottom-0 left-1/2 -ml-8 text-xs text-slate-500">Generation</div>
                      </div>
                    </div>

                    <div>
                      <div className="text-sm font-semibold text-slate-400 mb-2">Resource Store</div>
                      <div className="relative h-40 border border-slate-600 rounded bg-slate-800 p-2">
                        <svg className="w-full h-full" viewBox="0 0 100 100" preserveAspectRatio="none">
                          <line x1="0" y1="100" x2="100" y2="100" stroke="#475569" strokeWidth="0.5" vectorEffect="non-scaling-stroke" />
                          <line x1="0" y1="50" x2="100" y2="50" stroke="#475569" strokeWidth="0.5" vectorEffect="non-scaling-stroke" strokeDasharray="2" />
                          <line x1="0" y1="0" x2="100" y2="0" stroke="#475569" strokeWidth="0.5" vectorEffect="non-scaling-stroke" />
                          {history.map((h, i) => {
                            if (i === 0) return null;
                            const maxStore = Math.max(...history.map(h => h.resourceStore), initialEndowment);
                            const x1 = ((i - 1) / (history.length - 1)) * 100;
                            const x2 = (i / (history.length - 1)) * 100;
                            const y1 = (1 - history[i - 1].resourceStore / maxStore) * 100;
                            const y2 = (1 - h.resourceStore / maxStore) * 100;
                            return React.createElement('line', {
                              key: i,
                              x1: x1,
                              y1: y1,
                              x2: x2,
                              y2: y2,
                              stroke: "#3b82f6",
                              strokeWidth: "2",
                              vectorEffect: "non-scaling-stroke"
                            });
                          })}
                        </svg>
                        <div className="absolute left-0 top-0 text-xs text-slate-500">{Math.max(...history.map(h => h.resourceStore), initialEndowment).toFixed(0)}</div>
                        <div className="absolute left-0 bottom-0 text-xs text-slate-500">0</div>
                        <div className="absolute bottom-0 left-1/2 -ml-8 text-xs text-slate-500">Generation</div>
                      </div>
                    </div>
                  </div>
                </div>
              )}
            </div>
          );
        };

        const root = ReactDOM.createRoot(document.getElementById('root'));
        root.render(React.createElement(BitstringEvolution));
    </script>
</body>
