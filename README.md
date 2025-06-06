<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Sistema de Scoring de Crédito - Wizard</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            color: #333;
        }

        .wizard-container {
            max-width: 900px;
            margin: 0 auto;
            padding: 20px;
            min-height: 100vh;
            display: flex;
            flex-direction: column;
        }

        header {
            background: rgba(255, 255, 255, 0.95);
            backdrop-filter: blur(10px);
            padding: 30px;
            border-radius: 20px;
            text-align: center;
            margin-bottom: 30px;
            box-shadow: 0 8px 32px rgba(0, 0, 0, 0.1);
        }

        h1 {
            font-size: 2.5em;
            background: linear-gradient(135deg, #667eea, #764ba2);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            margin-bottom: 10px;
        }

        .subtitle {
            color: #666;
            font-size: 1.1em;
        }

        .progress-container {
            background: rgba(255, 255, 255, 0.9);
            padding: 20px;
            border-radius: 15px;
            margin-bottom: 30px;
            box-shadow: 0 4px 20px rgba(0, 0, 0, 0.1);
        }

        .progress-bar {
            display: flex;
            justify-content: space-between;
            align-items: center;
            position: relative;
            margin-bottom: 15px;
        }

        .progress-bar::before {
            content: '';
            position: absolute;
            top: 50%;
            left: 0;
            right: 0;
            height: 4px;
            background: #e0e0e0;
            z-index: 1;
            border-radius: 2px;
        }

        .progress-fill {
            position: absolute;
            top: 50%;
            left: 0;
            height: 4px;
            background: linear-gradient(135deg, #667eea, #764ba2);
            z-index: 2;
            border-radius: 2px;
            transition: width 0.5s ease;
            transform: translateY(-50%);
        }

        .step-indicator {
            width: 40px;
            height: 40px;
            border-radius: 50%;
            background: #e0e0e0;
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight: bold;
            color: #666;
            z-index: 3;
            position: relative;
            transition: all 0.3s ease;
        }

        .step-indicator.active {
            background: linear-gradient(135deg, #667eea, #764ba2);
            color: white;
            transform: scale(1.1);
        }

        .step-indicator.completed {
            background: #4CAF50;
            color: white;
        }

        .step-labels {
            display: flex;
            justify-content: space-between;
            margin-top: 10px;
        }

        .step-label {
            font-size: 0.9em;
            color: #666;
            text-align: center;
            flex: 1;
        }

        .step-label.active {
            color: #667eea;
            font-weight: bold;
        }

        .step-content {
            background: rgba(255, 255, 255, 0.95);
            backdrop-filter: blur(10px);
            padding: 40px;
            border-radius: 20px;
            box-shadow: 0 8px 32px rgba(0, 0, 0, 0.1);
            flex: 1;
            margin-bottom: 20px;
        }

        .step {
            display: none;
            animation: fadeIn 0.5s ease;
        }

        .step.active {
            display: block;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(20px); }
            to { opacity: 1; transform: translateY(0); }
        }

        .step-title {
            font-size: 1.8em;
            color: #333;
            margin-bottom: 10px;
            text-align: center;
        }

        .step-description {
            color: #666;
            text-align: center;
            margin-bottom: 30px;
            font-size: 1.1em;
        }

        .form-group {
            margin-bottom: 25px;
        }

        .form-group label {
            display: block;
            margin-bottom: 8px;
            font-weight: bold;
            color: #333;
        }

        .required::after {
            content: ' *';
            color: #e74c3c;
        }

        .form-group input {
            width: 100%;
            padding: 15px;
            border: 2px solid #e0e0e0;
            border-radius: 10px;
            font-size: 1em;
            transition: all 0.3s ease;
        }

        .form-group input:focus {
            outline: none;
            border-color: #667eea;
            box-shadow: 0 0 0 3px rgba(102, 126, 234, 0.1);
        }

        .parameter-group {
            background: #f8f9fa;
            padding: 25px;
            border-radius: 15px;
            margin-bottom: 25px;
            border-left: 5px solid #667eea;
        }

        .parameter-header {
            margin-bottom: 20px;
        }

        .parameter-name {
            font-size: 1.2em;
            font-weight: bold;
            color: #333;
            margin-bottom: 10px;
        }

        .parameter-info {
            display: flex;
            gap: 15px;
            align-items: center;
            flex-wrap: wrap;
        }

        .c-type {
            background: #667eea;
            color: white;
            padding: 4px 12px;
            border-radius: 20px;
            font-size: 0.85em;
            font-weight: 500;
        }

        .weight {
            background: #e74c3c;
            color: white;
            padding: 4px 10px;
            border-radius: 5px;
            font-size: 0.85em;
            font-weight: bold;
        }

        .radio-group {
            display: grid;
            gap: 12px;
        }

        .radio-option {
            display: flex;
            align-items: center;
            padding: 15px;
            background: white;
            border-radius: 10px;
            cursor: pointer;
            transition: all 0.3s ease;
            border: 2px solid transparent;
        }

        .radio-option:hover {
            background: #f0f4ff;
            transform: translateX(5px);
            border-color: #667eea;
        }

        .radio-option:has(input:checked) {
            background: #e8f2ff;
            border-color: #667eea;
            box-shadow: 0 4px 15px rgba(102, 126, 234, 0.2);
        }

        .radio-option input[type="radio"] {
            margin-right: 15px;
            transform: scale(1.3);
        }

        .radio-option label {
            cursor: pointer;
            flex: 1;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .points {
            background: #4CAF50;
            color: white;
            padding: 4px 10px;
            border-radius: 5px;
            font-size: 0.9em;
            font-weight: bold;
            min-width: 50px;
            text-align: center;
        }

        .navigation {
            display: flex;
            justify-content: space-between;
            align-items: center;
            background: rgba(255, 255, 255, 0.9);
            padding: 20px 30px;
            border-radius: 15px;
            box-shadow: 0 4px 20px rgba(0, 0, 0, 0.1);
        }

        .btn {
            padding: 12px 30px;
            border: none;
            border-radius: 8px;
            font-size: 1em;
            font-weight: bold;
            cursor: pointer;
            transition: all 0.3s ease;
            text-transform: uppercase;
            letter-spacing: 0.5px;
        }

        .btn-primary {
            background: linear-gradient(135deg, #667eea, #764ba2);
            color: white;
        }

        .btn-primary:hover:not(:disabled) {
            transform: translateY(-2px);
            box-shadow: 0 6px 20px rgba(102, 126, 234, 0.4);
        }

        .btn-secondary {
            background: #6c757d;
            color: white;
        }

        .btn-secondary:hover:not(:disabled) {
            background: #5a6268;
            transform: translateY(-2px);
        }

        .btn:disabled {
            background: #ccc !important;
            cursor: not-allowed !important;
            transform: none !important;
            box-shadow: none !important;
        }

        .results-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 20px;
            margin: 30px 0;
        }

        .result-card {
            background: linear-gradient(135deg, #667eea, #764ba2);
            color: white;
            padding: 25px;
            border-radius: 15px;
            text-align: center;
            box-shadow: 0 8px 25px rgba(0, 0, 0, 0.2);
        }

        .result-label {
            font-size: 0.9em;
            opacity: 0.9;
            margin-bottom: 10px;
        }

        .result-value {
            font-size: 2.2em;
            font-weight: bold;
        }

        .category-breakdown {
            background: #f8f9fa;
            padding: 25px;
            border-radius: 15px;
            margin-top: 30px;
        }

        .category-item {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 15px;
            padding: 15px;
            background: white;
            border-radius: 10px;
        }

        .category-progress {
            width: 200px;
            height: 20px;
            background: #e0e0e0;
            border-radius: 10px;
            overflow: hidden;
            margin: 0 15px;
        }

        .category-progress-fill {
            height: 100%;
            background: linear-gradient(135deg, #667eea, #764ba2);
            border-radius: 10px;
            transition: width 1s ease;
        }

        @media (max-width: 768px) {
            .wizard-container {
                padding: 10px;
            }

            h1 {
                font-size: 2em;
            }

            .step-content {
                padding: 25px;
            }

            .navigation {
                padding: 15px 20px;
            }

            .results-grid {
                grid-template-columns: 1fr;
            }

            .category-item {
                flex-direction: column;
                gap: 10px;
            }

            .category-progress {
                width: 100%;
            }

            .parameter-info {
                flex-direction: column;
                align-items: flex-start;
                gap: 10px;
            }
        }

        .cnpj-input {
            font-family: monospace;
            letter-spacing: 1px;
        }
    </style>
</head>
<body>
    <div class="wizard-container">
        <header>
            <h1>Sistema de Scoring de Crédito</h1>
            <p class="subtitle">Análise completa baseada nos 5 Cs do Crédito</p>
        </header>

        <!-- Progress Bar -->
        <div class="progress-container">
            <div class="progress-bar">
                <div class="progress-fill" id="progressFill"></div>
                <div class="step-indicator active" data-step="1">1</div>
                <div class="step-indicator" data-step="2">2</div>
                <div class="step-indicator" data-step="3">3</div>
                <div class="step-indicator" data-step="4">4</div>
                <div class="step-indicator" data-step="5">5</div>
                <div class="step-indicator" data-step="6">6</div>
            </div>
            <div class="step-labels">
                <div class="step-label active">Dados</div>
                <div class="step-label">Caráter</div>
                <div class="step-label">Capacidade</div>
                <div class="step-label">Capital</div>
                <div class="step-label">Condições</div>
                <div class="step-label">Resultados</div>
            </div>
        </div>

        <!-- Step Content -->
        <div class="step-content">
            <!-- Step 1: Dados da Empresa -->
            <div class="step active" id="step1">
                <h2 class="step-title">📊 Dados da Empresa</h2>
                <p class="step-description">Vamos começar com as informações básicas da sua empresa</p>
                
                <div class="form-group">
                    <label for="companyName" class="required">Nome da Empresa</label>
                    <input type="text" id="companyName" placeholder="Digite o nome completo da empresa" required>
                </div>

                <div class="form-group">
                    <label for="cnpj" class="required">CNPJ</label>
                    <input type="text" id="cnpj" class="cnpj-input" placeholder="00.000.000/0000-00" maxlength="18" required>
                </div>
                
                <div class="form-group">
                    <label for="revenue" class="required">Faturamento Anual (R$)</label>
                    <input type="number" id="revenue" placeholder="Ex: 1300000" min="1" required>
                </div>
            </div>

            <!-- Step 2: Caráter -->
            <div class="step" id="step2">
                <h2 class="step-title">🎯 Caráter</h2>
                <p class="step-description">Avaliação da reputação e histórico da empresa</p>
                
                <div class="parameter-group">
                    <div class="parameter-header">
                        <div class="parameter-name">Tempo de Atuação da Empresa</div>
                        <div class="parameter-info">
                            <span class="c-type">Caráter</span>
                            <span class="weight">Peso: 5%</span>
                        </div>
                    </div>
                    <div class="radio-group">
                        <div class="radio-option">
                            <input type="radio" id="tempo1" name="tempo" value="10">
                            <label for="tempo1">
                                <span>Maior que 3 e menor que 5 anos</span>
                                <span class="points">10 pts</span>
                            </label>
                        </div>
                        <div class="radio-option">
                            <input type="radio" id="tempo2" name="tempo" value="15">
                            <label for="tempo2">
                                <span>Maior que 5 e menor que 6 anos</span>
                                <span class="points">15 pts</span>
                            </label>
                        </div>
                        <div class="radio-option">
                            <input type="radio" id="tempo3" name="tempo" value="20">
                            <label for="tempo3">
                                <span>Maior que 6 e menor que 8 anos</span>
                                <span class="points">20 pts</span>
                            </label>
                        </div>
                        <div class="radio-option">
                            <input type="radio" id="tempo4" name="tempo" value="25">
                            <label for="tempo4">
                                <span>Maior que 8 e menor que 10 anos</span>
                                <span class="points">25 pts</span>
                            </label>
                        </div>
                        <div class="radio-option">
                            <input type="radio" id="tempo5" name="tempo" value="30">
                            <label for="tempo5">
                                <span>Maior que 10 anos</span>
                                <span class="points">30 pts</span>
                            </label>
                        </div>
                    </div>
                </div>

                <div class="parameter-group">
                    <div class="parameter-header">
                        <div class="parameter-name">Conceito da Empresa</div>
                        <div class="parameter-info">
                            <span class="c-type">Caráter</span>
                            <span class="weight">Peso: 15%</span>
                        </div>
                    </div>
                    <div class="radio-group">
                        <div class="radio-option">
                            <input type="radio" id="conceito1" name="conceito" value="10">
                            <label for="conceito1">
                                <span>Com até 3 restrições cadastrais esclarecidas e sem experiência</span>
                                <span class="points">10 pts</span>
                            </label>
                        </div>
                        <div class="radio-option">
                            <input type="radio" id="conceito2" name="conceito" value="15">
                            <label for="conceito2">
                                <span>Com até 3 restrições cadastrais esclarecidas e com experiência desfavorável</span>
                                <span class="points">15 pts</span>
                            </label>
                        </div>
                        <div class="radio-option">
                            <input type="radio" id="conceito3" name="conceito" value="20">
                            <label for="conceito3">
                                <span>Com até 3 restrições esclarecidas e com boa experiência</span>
                                <span class="points">20 pts</span>
                            </label>
                        </div>
                        <div class="radio-option">
                            <input type="radio" id="conceito4" name="conceito" value="25">
                            <label for="conceito4">
                                <span>Sem restrições e sem experiência</span>
                                <span class="points">25 pts</span>
                            </label>
                        </div>
                        <div class="radio-option">
                            <input type="radio" id="conceito5" name="conceito" value="30">
                            <label for="conceito5">
                                <span>Sem restrições e com boa experiência</span>
                                <span class="points">30 pts</span>
                            </label>
                        </div>
                    </div>
                </div>

                <div class="parameter-group">
                    <div class="parameter-header">
                        <div class="parameter-name">Patrimônio Pessoal do Garantidor</div>
                        <div class="parameter-info">
                            <span class="c-type">Colateral</span>
                            <span class="weight">Peso: 10%</span>
                        </div>
                    </div>
                    <div class="radio-group">
                        <div class="radio-option">
                            <input type="radio" id="patrimonio1" name="patrimonio" value="10">
                            <label for="patrimonio1">
                                <span>PP menor que 50% do crédito solicitado</span>
                                <span class="points">10 pts</span>
                            </label>
                        </div>
                        <div class="radio-option">
                            <input type="radio" id="patrimonio2" name="patrimonio" value="15">
                            <label for="patrimonio2">
                                <span>PP maior que 50% e menor que 75% do crédito</span>
                                <span class="points">15 pts</span>
                            </label>
                        </div>
                        <div class="radio-option">
                            <input type="radio" id="patrimonio3" name="patrimonio" value="20">
                            <label for="patrimonio3">
                                <span>PP maior que 75% e menor que 100% do crédito</span>
                                <span class="points">20 pts</span>
                            </label>
                        </div>
                        <div class="radio-option">
                            <input type="radio" id="patrimonio4" name="patrimonio" value="25">
                            <label for="patrimonio4">
                                <span>PP maior que 100% e menor que 120% do crédito</span>
                                <span class="points">25 pts</span>
                            </label>
                        </div>
                        <div class="radio-option">
                            <input type="radio" id="patrimonio5" name="patrimonio" value="30">
                            <label for="patrimonio5">
                                <span>PP maior que 120% do crédito solicitado</span>
                                <span class="points">30 pts</span>
                            </label>
                        </div>
                    </div>
                </div>
            </div>

            <!-- Step 3: Capacidade -->
            <div class="step" id="step3">
                <h2 class="step-title">💪 Capacidade</h2>
                <p class="step-description">Análise da capacidade financeira e operacional</p>
                
                <div class="parameter-group">
                    <div class="parameter-header">
                        <div class="parameter-name">Evolução do Faturamento</div>
                        <div class="parameter-info">
                            <span class="c-type">Capacidade</span>
                            <span class="weight">Peso: 5%</span>
                        </div>
                    </div>
                    <div class="radio-group">
                        <div class="radio-option">
                            <input type="radio" id="evolucao1" name="evolucao" value="10">
                            <label for="evolucao1">
                                <span>Involução maior que 3%</span>
                                <span class="points">10 pts</span>
                            </label>
                        </div>
                        <div class="radio-option">
                            <input type="radio" id="evolucao2" name="evolucao" value="15">
                            <label for="evolucao2">
                                <span>Involução entre 1 e 3%</span>
                                <span class="points">15 pts</span>
                            </label>
                        </div>
                        <div class="radio-option">
                            <input type="radio" id="evolucao3" name="evolucao" value="20">
                            <label for="evolucao3">
                                <span>Manteve-se estável (-1% a 1%)</span>
                                <span class="points">20 pts</span>
                            </label>
                        </div>
                        <div class="radio-option">
                            <input type="radio" id="evolucao4" name="evolucao" value="25">
                            <label for="evolucao4">
                                <span>Positiva entre 1 e 10%</span>
                                <span class="points">25 pts</span>
                            </label>
                        </div>
                        <div class="radio-option">
                            <input type="radio" id="evolucao5" name="evolucao" value="30">
                            <label for="evolucao5">
                                <span>Positiva maior que 10%</span>
                                <span class="points">30 pts</span>
                            </label>
                        </div>
                    </div>
                </div>

                <div class="parameter-group">
                    <div class="parameter-header">
                        <div class="parameter-name">Margem Operacional</div>
                        <div class="parameter-info">
                            <span class="c-type">Capacidade</span>
                            <span class="weight">Peso: 20%</span>
                        </div>
                    </div>
                    <div class="radio-group">
                        <div class="radio-option">
                            <input type="radio" id="margem1" name="margem" value="10">
                            <label for="margem1">
                                <span>Valor negativo maior que 5%</span>
                                <span class="points">10 pts</span>
                            </label>
                        </div>
                        <div class="radio-option">
                            <input type="radio" id="margem2" name="margem" value="15">
                            <label for="margem2">
                                <span>Valor negativo até 5%</span>
                                <span class="points">15 pts</span>
                            </label>
                        </div>
                        <div class="radio-option">
                            <input type="radio" id="margem3" name="margem" value="20">
                            <label for="margem3">
                                <span>Valor positivo entre 5% e 10%</span>
                                <span class="points">20 pts</span>
                            </label>
                        </div>
                        <div class="radio-option">
                            <input type="radio" id="margem4" name="margem" value="25">
                            <label for="margem4">
                                <span>Valor positivo entre 10% e 20%</span>
                                <span class="points">25 pts</span>
                            </label>
                        </div>
                        <div class="radio-option">
                            <input type="radio" id="margem5" name="margem" value="30">
                            <label for="margem5">
                                <span>Valor positivo maior que 20%</span>
                                <span class="points">30 pts</span>
                            </label>
                        </div>
                    </div>
                </div>

                <div class="parameter-group">
                    <div class="parameter-header">
                        <div class="parameter-name">Índice de Liquidez</div>
                        <div class="parameter-info">
                            <span class="c-type">Capacidade</span>
                            <span class="weight">Peso: 15%</span>
                        </div>
                    </div>
                    <div class="radio-group">
                        <div class="radio-option">
                            <input type="radio" id="liquidez1" name="liquidez" value="10">
                            <label for="liquidez1">
                                <span>Menor que 0,75</span>
                                <span class="points">10 pts</span>
                            </label>
                        </div>
                        <div class="radio-option">
                            <input type="radio" id="liquidez2" name="liquidez" value="15">
                            <label for="liquidez2">
                                <span>Entre 0,75 e 1,00</span>
                                <span class="points">15 pts</span>
                            </label>
                        </div>
                        <div class="radio-option">
                            <input type="radio" id="liquidez3" name="liquidez" value="20">
                            <label for="liquidez3">
                                <span>Entre 1,00 e 1,50</span>
                                <span class="points">20 pts</span>
                            </label>
                        </div>
                        <div class="radio-option">
                            <input type="radio" id="liquidez4" name="liquidez" value="25">
                            <label for="liquidez4">
                                <span>Entre 1,50 e 2,00</span>
                                <span class="points">25 pts</span>
                            </label>
                        </div>
                        <div class="radio-option">
                            <input type="radio" id="liquidez5" name="liquidez" value="30">
                            <label for="liquidez5">
                                <span>Maior que 2,00</span>
                                <span class="points">30 pts</span>
                            </label>
                        </div>
                    </div>
                </div>

                <div class="parameter-group">
                    <div class="parameter-header">
                        <div class="parameter-name">Ciclo Financeiro</div>
                        <div class="parameter-info">
                            <span class="c-type">Capacidade</span>
                            <span class="weight">Peso: 7%</span>
                        </div>
                    </div>
                    <div class="radio-group">
                        <div class="radio-option">
                            <input type="radio" id="ciclo1" name="ciclo" value="10">
                            <label for="ciclo1">
                                <span>Maior que 45 dias</span>
                                <span class="points">10 pts</span>
                            </label>
                        </div>
                        <div class="radio-option">
                            <input type="radio" id="ciclo2" name="ciclo" value="15">
                            <label for="ciclo2">
                                <span>Entre 30 e 45 dias</span>
                                <span class="points">15 pts</span>
                            </label>
                        </div>
                        <div class="radio-option">
                            <input type="radio" id="ciclo3" name="ciclo" value="20">
                            <label for="ciclo3">
                                <span>Entre 10 e 30 dias</span>
                                <span class="points">20 pts</span>
                            </label>
                        </div>
                        <div class="radio-option">
                            <input type="radio" id="ciclo4" name="ciclo" value="25">
                            <label for="ciclo4">
                                <span>Entre -10 e 10 dias</span>
                                <span class="points">25 pts</span>
                            </label>
                        </div>
                        <div class="radio-option">
                            <input type="radio" id="ciclo5" name="ciclo" value="30">
                            <label for="ciclo5">
                                <span>Menor que -10 dias</span>
                                <span class="points">30 pts</span>
                            </label>
                        </div>
                    </div>
                </div>
            </div>

            <!-- Step 4: Capital -->
            <div class="step" id="step4">
                <h2 class="step-title">💰 Capital</h2>
                <p class="step-description">Avaliação da estrutura de capital da empresa</p>
                
                <div class="parameter-group">
                    <div class="parameter-header">
                        <div class="parameter-name">Índice de Alavancagem</div>
                        <div class="parameter-info">
                            <span class="c-type">Capital</span>
                            <span class="weight">Peso: 10%</span>
                        </div>
                    </div>
                    <div class="radio-group">
                        <div class="radio-option">
                            <input type="radio" id="alavancagem1" name="alavancagem" value="10">
                            <label for="alavancagem1">
                                <span>Maior que 1,50</span>
                                <span class="points">10 pts</span>
                            </label>
                        </div>
                        <div class="radio-option">
                            <input type="radio" id="alavancagem2" name="alavancagem" value="15">
                            <label for="alavancagem2">
                                <span>Entre 1,20 e 1,50</span>
                                <span class="points">15 pts</span>
                            </label>
                        </div>
                        <div class="radio-option">
                            <input type="radio" id="alavancagem3" name="alavancagem" value="20">
                            <label for="alavancagem3">
                                <span>Entre 1,00 e 1,20</span>
                                <span class="points">20 pts</span>
                            </label>
                        </div>
                        <div class="radio-option">
                            <input type="radio" id="alavancagem4" name="alavancagem" value="25">
                            <label for="alavancagem4">
                                <span>Entre 0,75 e 1,00</span>
                                <span class="points">25 pts</span>
                            </label>
                        </div>
                        <div class="radio-option">
                            <input type="radio" id="alavancagem5" name="alavancagem" value="30">
                            <label for="alavancagem5">
                                <span>Menor que 0,75</span>
                                <span class="points">30 pts</span>
                            </label>
                        </div>
                    </div>
                </div>
            </div>

            <!-- Step 5: Condições -->
            <div class="step" id="step5">
                <h2 class="step-title">🔄 Condições</h2>
                <p class="step-description">Análise das condições de mercado e operacionais</p>
                
                <div class="parameter-group">
                    <div class="parameter-header">
                        <div class="parameter-name">Concentração das Vendas</div>
                        <div class="parameter-info">
                            <span class="c-type">Condições</span>
                            <span class="weight">Peso: 10%</span>
                        </div>
                    </div>
                    <div class="radio-group">
                        <div class="radio-option">
                            <input type="radio" id="concentracao1" name="concentracao" value="10">
                            <label for="concentracao1">
                                <span>Acima de 80% em 1 cliente</span>
                                <span class="points">10 pts</span>
                            </label>
                        </div>
                        <div class="radio-option">
                            <input type="radio" id="concentracao2" name="concentracao" value="15">
                            <label for="concentracao2">
                                <span>Entre 50% e 80% em 1 cliente</span>
                                <span class="points">15 pts</span>
                            </label>
                        </div>
                        <div class="radio-option">
                            <input type="radio" id="concentracao3" name="concentracao" value="20">
                            <label for="concentracao3">
                                <span>Entre 30% e 50% em 1 cliente</span>
                                <span class="points">20 pts</span>
                            </label>
                        </div>
                        <div class="radio-option">
                            <input type="radio" id="concentracao4" name="concentracao" value="25">
                            <label for="concentracao4">
                                <span>Entre 15% e 30% em 1 cliente</span>
                                <span class="points">25 pts</span>
                            </label>
                        </div>
                        <div class="radio-option">
                            <input type="radio" id="concentracao5" name="concentracao" value="30">
                            <label for="concentracao5">
                                <span>Nenhum cliente concentra mais que 15%</span>
                                <span class="points">30 pts</span>
                            </label>
                        </div>
                    </div>
                </div>

                <div class="parameter-group">
                    <div class="parameter-header">
                        <div class="parameter-name">Dependência de Fornecedores</div>
                        <div class="parameter-info">
                            <span class="c-type">Condições</span>
                            <span class="weight">Peso: 3%</span>
                        </div>
                    </div>
                    <div class="radio-group">
                        <div class="radio-option">
                            <input type="radio" id="dependencia1" name="dependencia" value="10">
                            <label for="dependencia1">
                                <span>Mais de 80% de 1 fornecedor</span>
                                <span class="points">10 pts</span>
                            </label>
                        </div>
                        <div class="radio-option">
                            <input type="radio" id="dependencia2" name="dependencia" value="15">
                            <label for="dependencia2">
                                <span>Entre 50% e 80% de 1 fornecedor</span>
                                <span class="points">15 pts</span>
                            </label>
                        </div>
                        <div class="radio-option">
                            <input type="radio" id="dependencia3" name="dependencia" value="20">
                            <label for="dependencia3">
                                <span>Entre 30% e 50% de 1 fornecedor</span>
                                <span class="points">20 pts</span>
                            </label>
                        </div>
                        <div class="radio-option">
                            <input type="radio" id="dependencia4" name="dependencia" value="25">
                            <label for="dependencia4">
                                <span>Entre 15% e 30% de 1 fornecedor</span>
                                <span class="points">25 pts</span>
                            </label>
                        </div>
                        <div class="radio-option">
                            <input type="radio" id="dependencia5" name="dependencia" value="30">
                            <label for="dependencia5">
                                <span>Menos de 15% de 1 fornecedor</span>
                                <span class="points">30 pts</span>
                            </label>
                        </div>
                    </div>
                </div>
            </div>

            <!-- Step 6: Resultados -->
            <div class="step" id="step6">
                <h2 class="step-title">🎉 Resultados da Análise</h2>
                <p class="step-description">Confira o score de crédito calculado</p>
                
                <div class="results-grid">
                    <div class="result-card">
                        <div class="result-label">Pontuação Total</div>
                        <div class="result-value" id="totalScore">0</div>
                    </div>
                    <div class="result-card">
                        <div class="result-label">Percentual</div>
                        <div class="result-value" id="scorePercentage">0%</div>
                    </div>
                    <div class="result-card">
                        <div class="result-label">Limite de Crédito</div>
                        <div class="result-value" id="creditLimit">R$ 0</div>
                    </div>
                    <div class="result-card">
                        <div class="result-label">Classificação</div>
                        <div class="result-value" id="riskClass">-</div>
                    </div>
                </div>

                <div class="category-breakdown">
                    <h3 style="margin-bottom: 20px; text-align: center;">📊 Detalhamento por Categoria</h3>
                    <div id="categoryBreakdown"></div>
                </div>
            </div>
        </div>

        <!-- Navigation -->
        <div class="navigation">
            <button class="btn btn-secondary" id="prevBtn" onclick="previousStep()" disabled>
                ← Anterior
            </button>
            <div style="text-align: center;">
                <span id="stepInfo">Passo 1 de 6</span>
            </div>
            <button class="btn btn-primary" id="nextBtn" onclick="nextStep()">
                Próximo →
            </button>
        </div>
    </div>

    <script>
        // Estado atual do wizard
        let currentStep = 1;
        const totalSteps = 6;

        // Definição dos pesos
        const weights = {
            tempo: 0.05,
            conceito: 0.15,
            patrimonio: 0.10,
            evolucao: 0.05,
            concentracao: 0.10,
            dependencia: 0.03,
            margem: 0.20,
            liquidez: 0.15,
            ciclo: 0.07,
            alavancagem: 0.10
        };

        // Mapeamento para categorias
        const parameterToC = {
            tempo: 'Caráter',
            conceito: 'Caráter',
            patrimonio: 'Colateral',
            evolucao: 'Capacidade',
            concentracao: 'Condições',
            dependencia: 'Condições',
            margem: 'Capacidade',
            liquidez: 'Capacidade',
            ciclo: 'Capacidade',
            alavancagem: 'Capital'
        };

        // Inicialização
        document.addEventListener('DOMContentLoaded', function() {
            updateStepDisplay();
            updateNavigation();
            setupCNPJMask();
        });

        // Formatação do CNPJ
        function setupCNPJMask() {
            const cnpjInput = document.getElementById('cnpj');
            cnpjInput.addEventListener('input', function(e) {
                let value = e.target.value.replace(/\D/g, '');
                if (value.length <= 14) {
                    value = value.replace(/^(\d{2})(\d)/, '$1.$2');
                    value = value.replace(/^(\d{2})\.(\d{3})(\d)/, '$1.$2.$3');
                    value = value.replace(/\.(\d{3})(\d)/, '.$1/$2');
                    value = value.replace(/(\d{4})(\d)/, '$1-$2');
                    e.target.value = value;
                }
            });
        }

        // Validação de CNPJ
        function validateCNPJ(cnpj) {
            cnpj = cnpj.replace(/[^\d]+/g, '');
            if (cnpj.length !== 14) return false;
            
            // Elimina CNPJs inválidos conhecidos
            if (/^(\d)\1+$/.test(cnpj)) return false;
            
            // Valida 1º dígito verificador
            let tamanho = cnpj.length - 2;
            let numeros = cnpj.substring(0, tamanho);
            let digitos = cnpj.substring(tamanho);
            let soma = 0;
            let pos = tamanho - 7;
            
            for (let i = tamanho; i >= 1; i--) {
                soma += numeros.charAt(tamanho - i) * pos--;
                if (pos < 2) pos = 9;
            }
            
            let resultado = soma % 11 < 2 ? 0 : 11 - soma % 11;
            if (resultado != digitos.charAt(0)) return false;
            
            // Valida 2º dígito verificador
            tamanho = tamanho + 1;
            numeros = cnpj.substring(0, tamanho);
            soma = 0;
            pos = tamanho - 7;
            
            for (let i = tamanho; i >= 1; i--) {
                soma += numeros.charAt(tamanho - i) * pos--;
                if (pos < 2) pos = 9;
            }
            
            resultado = soma % 11 < 2 ? 0 : 11 - soma % 11;
            return resultado == digitos.charAt(1);
        }

        // Navegação entre passos
        function nextStep() {
            if (validateCurrentStep()) {
                if (currentStep < totalSteps) {
                    currentStep++;
                    if (currentStep === 6) {
                        calculateScore();
                    }
                    updateStepDisplay();
                    updateNavigation();
                    updateProgress();
                }
            }
        }

        function previousStep() {
            if (currentStep > 1) {
                currentStep--;
                updateStepDisplay();
                updateNavigation();
                updateProgress();
            }
        }

        // Validação do passo atual
        function validateCurrentStep() {
            switch(currentStep) {
                case 1:
                    const companyName = document.getElementById('companyName').value.trim();
                    const cnpj = document.getElementById('cnpj').value.trim();
                    const revenue = document.getElementById('revenue').value;
                    
                    if (!companyName) {
                        alert('Por favor, informe o nome da empresa.');
                        document.getElementById('companyName').focus();
                        return false;
                    }
                    if (!cnpj) {
                        alert('Por favor, informe o CNPJ.');
                        document.getElementById('cnpj').focus();
                        return false;
                    }
                    if (!validateCNPJ(cnpj)) {
                        alert('Por favor, informe um CNPJ válido.');
                        document.getElementById('cnpj').focus();
                        return false;
                    }
                    if (!revenue || revenue <= 0) {
                        alert('Por favor, informe um faturamento válido.');
                        document.getElementById('revenue').focus();
                        return false;
                    }
                    return true;

                case 2:
                    const requiredParams2 = ['tempo', 'conceito', 'patrimonio'];
                    return validateParameters(requiredParams2);

                case 3:
                    const requiredParams3 = ['evolucao', 'margem', 'liquidez', 'ciclo'];
                    return validateParameters(requiredParams3);

                case 4:
                    const requiredParams4 = ['alavancagem'];
                    return validateParameters(requiredParams4);

                case 5:
                    const requiredParams5 = ['concentracao', 'dependencia'];
                    return validateParameters(requiredParams5);

                default:
                    return true;
            }
        }

        function validateParameters(paramList) {
            for (let param of paramList) {
                if (!document.querySelector(`input[name="${param}"]:checked`)) {
                    alert(`Por favor, selecione uma opção para o parâmetro: ${getParameterName(param)}`);
                    return false;
                }
            }
            return true;
        }

        function getParameterName(param) {
            const names = {
                tempo: 'Tempo de Atuação',
                conceito: 'Conceito',
                patrimonio: 'Patrimônio Pessoal',
                evolucao: 'Evolução do Faturamento',
                margem: 'Margem Operacional',
                liquidez: 'Liquidez',
                ciclo: 'Ciclo Financeiro',
                alavancagem: 'Alavancagem',
                concentracao: 'Concentração das Vendas',
                dependencia: 'Dependência de Fornecedores'
            };
            return names[param] || param;
        }

        // Atualização da interface
        function updateStepDisplay() {
            // Esconder todos os passos
            document.querySelectorAll('.step').forEach(step => {
                step.classList.remove('active');
            });
            
            // Mostrar passo atual
            document.getElementById(`step${currentStep}`).classList.add('active');
        }

        function updateNavigation() {
            const prevBtn = document.getElementById('prevBtn');
            const nextBtn = document.getElementById('nextBtn');
            const stepInfo = document.getElementById('stepInfo');
            
            // Atualizar botão anterior
            prevBtn.disabled = currentStep === 1;
            
            // Atualizar botão próximo
            if (currentStep === totalSteps) {
                nextBtn.style.display = 'none';
            } else {
                nextBtn.style.display = 'block';
                nextBtn.textContent = 'Próximo →';
            }
            
            // Atualizar informação do passo
            stepInfo.textContent = `Passo ${currentStep} de ${totalSteps}`;
        }

        function updateProgress() {
            const progressFill = document.getElementById('progressFill');
            const progressPercentage = ((currentStep - 1) / (totalSteps - 1)) * 100;
            progressFill.style.width = progressPercentage + '%';
            
            // Atualizar indicadores
            document.querySelectorAll('.step-indicator').forEach((indicator, index) => {
                const stepNumber = index + 1;
                indicator.classList.remove('active', 'completed');
                
                if (stepNumber === currentStep) {
                    indicator.classList.add('active');
                } else if (stepNumber < currentStep) {
                    indicator.classList.add('completed');
                }
            });
            
            // Atualizar labels
            document.querySelectorAll('.step-label').forEach((label, index) => {
                const stepNumber = index + 1;
                label.classList.remove('active');
                
                if (stepNumber === currentStep) {
                    label.classList.add('active');
                }
            });
        }

        // Cálculo do score
        function calculateScore() {
            // Obter valores selecionados
            const parameters = {
                tempo: parseInt(document.querySelector('input[name="tempo"]:checked')?.value || 0),
                conceito: parseInt(document.querySelector('input[name="conceito"]:checked')?.value || 0),
                patrimonio: parseInt(document.querySelector('input[name="patrimonio"]:checked')?.value || 0),
                evolucao: parseInt(document.querySelector('input[name="evolucao"]:checked')?.value || 0),
                concentracao: parseInt(document.querySelector('input[name="concentracao"]:checked')?.value || 0),
                dependencia: parseInt(document.querySelector('input[name="dependencia"]:checked')?.value || 0),
                margem: parseInt(document.querySelector('input[name="margem"]:checked')?.value || 0),
                liquidez: parseInt(document.querySelector('input[name="liquidez"]:checked')?.value || 0),
                ciclo: parseInt(document.querySelector('input[name="ciclo"]:checked')?.value || 0),
                alavancagem: parseInt(document.querySelector('input[name="alavancagem"]:checked')?.value || 0)
            };

            // Calcular pontuação ponderada
            let totalScore = 0;
            let categoryScores = {
                'Caráter': 0,
                'Capacidade': 0,
                'Capital': 0,
                'Colateral': 0,
                'Condições': 0
            };

            for (const [param, value] of Object.entries(parameters)) {
                const weightedScore = (value * weights[param]);
                totalScore += weightedScore;
                
                // Adicionar ao score da categoria correspondente
                const category = parameterToC[param];
                categoryScores[category] += weightedScore;
            }

            // Obter faturamento
            const revenue = parseFloat(document.getElementById('revenue').value) || 0;

            // Calcular percentual e limite
            const scorePercentage = totalScore / 30;
            const limitPercentage = Math.min(scorePercentage, 0.10);
            const creditLimit = revenue * limitPercentage / 12;

            // Classificação de risco
            const riskClass = getRiskClassification(scorePercentage);

            // Exibir resultados
            document.getElementById('totalScore').textContent = totalScore.toFixed(2);
            document.getElementById('scorePercentage').textContent = (scorePercentage * 100).toFixed(1) + '%';
            document.getElementById('creditLimit').textContent = 'R$ ' + creditLimit.toLocaleString('pt-BR', { minimumFractionDigits: 2, maximumFractionDigits: 2 });
            document.getElementById('riskClass').textContent = riskClass.classe;

            // Exibir detalhamento por categoria
            displayCategoryBreakdown(categoryScores);
        }

        function getRiskClassification(scorePercentage) {
            if (scorePercentage >= 0.8) return { classe: 'Baixo Risco', cor: '#27ae60' };
            if (scorePercentage >= 0.6) return { classe: 'Risco Moderado', cor: '#f39c12' };
            if (scorePercentage >= 0.4) return { classe: 'Risco Médio', cor: '#e67e22' };
            return { classe: 'Alto Risco', cor: '#e74c3c' };
        }

        function displayCategoryBreakdown(categoryScores) {
            let categoryHTML = '';
            for (const [category, score] of Object.entries(categoryScores)) {
                const maxScore = getMaxScoreForCategory(category);
                const percentage = (score / maxScore) * 100;
                categoryHTML += `
                    <div class="category-item">
                        <span><strong>${category}</strong></span>
                        <div class="category-progress">
                            <div class="category-progress-fill" style="width: ${percentage}%"></div>
                        </div>
                        <span>${score.toFixed(2)} / ${maxScore.toFixed(2)}</span>
                    </div>
                `;
            }
            document.getElementById('categoryBreakdown').innerHTML = categoryHTML;
        }

        function getMaxScoreForCategory(category) {
            let maxScore = 0;
            for (const [param, cat] of Object.entries(parameterToC)) {
                if (cat === category) {
                    maxScore += 30 * weights[param];
                }
            }
            return maxScore;
        }

        // Funcionalidades extras
        function exportarRelatorio() {
            const companyName = document.getElementById('companyName').value;
            const cnpj = document.getElementById('cnpj').value;
            const revenue = document.getElementById('revenue').value;
            const totalScore = document.getElementById('totalScore').textContent;
            const scorePercentage = document.getElementById('scorePercentage').textContent;
            const creditLimit = document.getElementById('creditLimit').textContent;
            const riskClass = document.getElementById('riskClass').textContent;

            const reportContent = `
RELATÓRIO DE SCORING DE CRÉDITO
===============================

DADOS DA EMPRESA:
• Nome: ${companyName}
• CNPJ: ${cnpj}
• Faturamento Anual: R$ ${parseFloat(revenue).toLocaleString('pt-BR')}

RESULTADOS:
• Pontuação Total: ${totalScore}
• Percentual do Score: ${scorePercentage}
• Limite de Crédito Sugerido: ${creditLimit}
• Classificação de Risco: ${riskClass}

Data: ${new Date().toLocaleDateString('pt-BR')}
            `;

            const blob = new Blob([reportContent], { type: 'text/plain' });
            const url = URL.createObjectURL(blob);
            const a = document.createElement('a');
            a.href = url;
            a.download = `relatorio_scoring_${companyName.replace(/\s+/g, '_')}.txt`;
            document.body.appendChild(a);
            a.click();
            document.body.removeChild(a);
            URL.revokeObjectURL(url);
        }

        function reiniciarAnalise() {
            if (confirm('Deseja realmente reiniciar a análise? Todos os dados serão perdidos.')) {
                currentStep = 1;
                document.getElementById('companyName').value = '';
                document.getElementById('cnpj').value = '';
                document.getElementById('revenue').value = '';
                
                // Limpar todas as seleções
                document.querySelectorAll('input[type="radio"]:checked').forEach(radio => {
                    radio.checked = false;
                });
                
                updateStepDisplay();
                updateNavigation();
                updateProgress();
            }
        }
    </script>
</body>
</html>
