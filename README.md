# AmazonDash
AmazonDash
<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Sistema de Gerenciamento de Dados</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <style>
        /* VARIÁVEIS CSS */
        :root {
            --primary: #2c3e50;
            --secondary: #3498db;
            --success: #27ae60;
            --warning: #f39c12;
            --danger: #e74c3c;
            --light: #ecf0f1;
            --dark: #2c3e50;
        }

        /* ESTILOS GERAIS */
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }

        body {
            background-color: #f5f7fa;
            color: #333;
            overflow-x: hidden;
        }

        /* TELA DE CARREGAMENTO */
        .loading-screen {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: linear-gradient(135deg, var(--primary) 0%, var(--secondary) 100%);
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            color: white;
            z-index: 9999;
        }

        .loading-spinner {
            width: 50px;
            height: 50px;
            border: 5px solid rgba(255, 255, 255, 0.3);
            border-radius: 50%;
            border-top-color: white;
            animation: spin 1s linear infinite;
            margin-bottom: 20px;
        }

        @keyframes spin {
            to { transform: rotate(360deg); }
        }

        @keyframes fadeIn {
            from { opacity: 0; }
            to { opacity: 1; }
        }

        /* MODAL DE LOGIN */
        .login-modal {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: linear-gradient(135deg, var(--primary) 0%, var(--secondary) 100%);
            display: flex;
            align-items: center;
            justify-content: center;
            z-index: 1000;
        }

        .login-container {
            background: white;
            border-radius: 10px;
            padding: 40px;
            width: 90%;
            max-width: 400px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.2);
        }

        .login-logo {
            text-align: center;
            margin-bottom: 30px;
        }

        .login-logo i {
            font-size: 3rem;
            color: var(--secondary);
            margin-bottom: 15px;
        }

        .login-logo h2 {
            color: var(--primary);
            margin-bottom: 5px;
        }

        .login-logo p {
            color: #666;
            font-size: 0.9rem;
        }

        .login-alert {
            display: none;
            padding: 10px 15px;
            background-color: #f8d7da;
            color: #721c24;
            border-radius: 5px;
            margin-bottom: 20px;
            font-size: 0.9rem;
        }

        .login-alert i {
            margin-right: 8px;
        }

        /* FORMULÁRIOS */
        .form-group {
            margin-bottom: 20px;
        }

        .form-label {
            display: block;
            margin-bottom: 8px;
            font-weight: 600;
            color: #555;
        }

        .form-label i {
            margin-right: 8px;
            color: var(--secondary);
        }

        .form-control {
            width: 100%;
            padding: 12px 15px;
            border: 2px solid #e0e0e0;
            border-radius: 8px;
            font-size: 1rem;
            transition: border-color 0.3s;
        }

        .form-control:focus {
            outline: none;
            border-color: var(--secondary);
        }

        /* BOTÕES */
        .btn {
            padding: 12px 24px;
            border: none;
            border-radius: 8px;
            cursor: pointer;
            font-size: 1rem;
            font-weight: 600;
            transition: all 0.3s;
            display: inline-flex;
            align-items: center;
            justify-content: center;
        }

        .btn i {
            margin-right: 8px;
        }

        .btn-primary {
            background-color: var(--secondary);
            color: white;
        }

        .btn-primary:hover {
            background-color: #2980b9;
            transform: translateY(-2px);
            box-shadow: 0 5px 15px rgba(52, 152, 219, 0.3);
        }

        .btn-success {
            background-color: var(--success);
            color: white;
        }

        .btn-success:hover {
            background-color: #219653;
            transform: translateY(-2px);
        }

        .btn-block {
            width: 100%;
        }

        /* LAYOUT PRINCIPAL */
        .container {
            display: flex;
            min-height: 100vh;
        }

        /* SIDEBAR */
        .sidebar {
            width: 250px;
            background-color: var(--primary);
            color: white;
            padding: 20px 0;
        }

        .sidebar-header {
            padding: 0 20px 20px;
            border-bottom: 1px solid rgba(255, 255, 255, 0.1);
        }

        .logo {
            display: flex;
            align-items: center;
            font-size: 1.5rem;
            font-weight: bold;
        }

        .logo i {
            margin-right: 10px;
            color: var(--secondary);
        }

        .nav-menu {
            list-style: none;
            margin-top: 20px;
        }

        .nav-item {
            margin-bottom: 5px;
        }

        .nav-link {
            display: flex;
            align-items: center;
            padding: 12px 20px;
            color: rgba(255, 255, 255, 0.8);
            text-decoration: none;
            transition: all 0.3s;
        }

        .nav-link:hover, .nav-link.active {
            background-color: rgba(255, 255, 255, 0.1);
            color: white;
        }

        .nav-link i {
            margin-right: 10px;
            width: 20px;
            text-align: center;
        }

        /* CONTEÚDO PRINCIPAL */
        .main-content {
            flex: 1;
            padding: 20px;
            overflow-y: auto;
        }

        .header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 30px;
            padding-bottom: 20px;
            border-bottom: 1px solid #e0e0e0;
        }

        .header h1 {
            color: var(--primary);
        }

        .user-info {
            display: flex;
            align-items: center;
            gap: 15px;
        }

        .user-avatar {
            width: 50px;
            height: 50px;
            background-color: var(--secondary);
            color: white;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight: bold;
            font-size: 1.2rem;
        }

        .user-name {
            font-weight: 600;
            color: var(--primary);
        }

        .user-role {
            font-size: 0.9rem;
            color: #666;
        }

        .access-badge {
            display: inline-block;
            padding: 3px 8px;
            border-radius: 12px;
            font-size: 0.8rem;
            margin-top: 3px;
        }

        /* PÁGINAS */
        .page {
            display: none;
            animation: fadeIn 0.5s ease;
        }

        .page.active {
            display: block;
        }

        /* DASHBOARD */
        .dashboard-welcome {
            text-align: center;
            margin-bottom: 30px;
            padding: 30px;
            background: linear-gradient(135deg, var(--primary) 0%, var(--secondary) 100%);
            border-radius: 10px;
            color: white;
        }

        .dashboard-welcome h2 {
            margin-bottom: 10px;
            font-size: 2rem;
        }

        .cards-container {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
            gap: 20px;
            margin-bottom: 30px;
        }

        .card {
            background: white;
            border-radius: 10px;
            padding: 20px;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.05);
            transition: transform 0.3s;
        }

        .card:hover {
            transform: translateY(-5px);
        }

        .card-title {
            font-size: 0.9rem;
            color: #666;
            margin-bottom: 10px;
        }

        .card-value {
            font-size: 2rem;
            font-weight: bold;
            color: var(--primary);
            margin-bottom: 10px;
        }

        .card-footer {
            font-size: 0.85rem;
            color: #888;
        }

        .success-text {
            color: var(--success);
        }

        .warning-text {
            color: var(--warning);
        }

        .danger-text {
            color: var(--danger);
        }

        .dashboard-container {
            display: grid;
            grid-template-columns: 2fr 1fr;
            gap: 20px;
        }

        @media (max-width: 992px) {
            .dashboard-container {
                grid-template-columns: 1fr;
            }
        }

        /* GRÁFICOS */
        .chart-container {
            background: white;
            border-radius: 10px;
            padding: 20px;
            margin-bottom: 20px;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.05);
        }

        .chart-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 20px;
        }

        .chart-title {
            font-weight: 600;
            color: var(--primary);
        }

        .fechamento-summary {
            background: white;
            border-radius: 10px;
            padding: 20px;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.05);
        }

        .summary-item {
            display: flex;
            justify-content: space-between;
            padding: 10px 0;
            border-bottom: 1px solid #f0f0f0;
        }

        .summary-label {
            color: #666;
        }

        .summary-value {
            font-weight: 600;
            color: var(--primary);
        }

        /* TABELAS */
        .table-container {
            background: white;
            border-radius: 10px;
            padding: 20px;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.05);
            overflow-x: auto;
        }

        .table {
            width: 100%;
            border-collapse: collapse;
        }

        .table th {
            background-color: #f8f9fa;
            padding: 12px 15px;
            text-align: left;
            font-weight: 600;
            color: var(--primary);
            border-bottom: 2px solid #e0e0e0;
        }

        .table td {
            padding: 12px 15px;
            border-bottom: 1px solid #f0f0f0;
        }

        .table tr:hover {
            background-color: #f8f9fa;
        }

        /* BADGES */
        .badge {
            display: inline-block;
            padding: 4px 8px;
            border-radius: 4px;
            font-size: 0.8rem;
            font-weight: 600;
        }

        .badge-success {
            background-color: #d4edda;
            color: #155724;
        }

        .badge-danger {
            background-color: #f8d7da;
            color: #721c24;
        }

        /* ALERTAS */
        .alert {
            padding: 15px;
            border-radius: 8px;
            margin-bottom: 20px;
            display: flex;
            align-items: center;
        }

        .alert-success {
            background-color: #d4edda;
            color: #155724;
            border: 1px solid #c3e6cb;
        }

        .alert-warning {
            background-color: #fff3cd;
            color: #856404;
            border: 1px solid #ffeaa7;
        }

        .alert-danger {
            background-color: #f8d7da;
            color: #721c24;
            border: 1px solid #f5c6cb;
        }

        .alert-info {
            background-color: #d1ecf1;
            color: #0c5460;
            border: 1px solid #bee5eb;
        }

        .alert i {
            margin-right: 10px;
        }

        /* FORMULÁRIOS ESPECÍFICOS */
        .form-container {
            background: white;
            border-radius: 10px;
            padding: 20px;
            margin-bottom: 20px;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.05);
        }

        .form-title {
            margin-bottom: 20px;
            color: var(--primary);
        }

        .fechamento-form {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 15px;
        }

        .fechamento-group {
            display: flex;
            flex-direction: column;
        }

        .fechamento-label {
            margin-bottom: 5px;
            font-weight: 600;
            color: #555;
        }

        .fechamento-input {
            padding: 10px 12px;
            border: 2px solid #e0e0e0;
            border-radius: 5px;
            font-size: 1rem;
        }

        .fechamento-input:focus {
            outline: none;
            border-color: var(--secondary);
        }

        /* TABS */
        .tabs-container {
            margin: 20px 0;
        }

        .tabs {
            display: flex;
            border-bottom: 2px solid #e0e0e0;
            margin-bottom: 20px;
        }

        .tab {
            padding: 12px 24px;
            cursor: pointer;
            border: none;
            background: none;
            font-weight: 600;
            color: #666;
            border-bottom: 3px solid transparent;
            transition: all 0.3s;
        }

        .tab:hover {
            color: var(--secondary);
        }

        .tab.active {
            color: var(--secondary);
            border-bottom: 3px solid var(--secondary);
        }

        .tab-content {
            display: none;
            animation: fadeIn 0.5s ease;
        }

        .tab-content.active {
            display: block;
        }

        /* GRID DE CONVÊNIOS */
        .convenios-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
            gap: 15px;
            margin-top: 15px;
        }

        .convenio-card {
            border: 2px solid #e0e0e0;
            border-radius: 8px;
            padding: 15px;
            cursor: pointer;
            transition: all 0.3s;
            background: white;
        }

        .convenio-card:hover {
            border-color: var(--secondary);
            transform: translateY(-2px);
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.1);
        }

        .convenio-card.selected {
            border-color: var(--success);
            background-color: rgba(39, 174, 96, 0.05);
        }

        .convenio-nome {
            font-weight: 600;
            margin-bottom: 5px;
            color: var(--primary);
        }

        .convenio-documento {
            font-size: 0.9rem;
            color: #666;
        }

        .convenio-valor-input {
            margin-top: 10px;
        }

        .convenio-valor-input input {
            width: 100%;
            padding: 8px 12px;
            border: 1px solid #ddd;
            border-radius: 5px;
            font-size: 0.9rem;
        }

        /* BADGE PARA CONVÊNIOS */
        .convenio-badge {
            display: inline-flex;
            align-items: center;
            padding: 5px 10px;
            background: #e3f2fd;
            color: #1565c0;
            border-radius: 20px;
            margin: 2px;
            font-size: 0.8rem;
        }

        .convenio-badge i {
            margin-right: 5px;
        }

        /* TOTAIS */
        .totals-container {
            background: #f8f9fa;
            border-radius: 8px;
            padding: 20px;
        }

        .total-item {
            display: flex;
            justify-content: space-between;
            padding: 10px 0;
            border-bottom: 1px solid #e0e0e0;
        }

        .total-item:last-child {
            border-bottom: none;
        }

        .total-label {
            color: #666;
        }

        .total-value {
            font-weight: 600;
            color: var(--primary);
        }

        /* UTILITÁRIOS */
        .d-flex {
            display: flex;
        }

        .justify-between {
            justify-content: space-between;
        }

        .justify-center {
            justify-content: center;
        }

        .align-center {
            align-items: center;
        }

        .text-center {
            text-align: center;
        }

        .text-right {
            text-align: right;
        }

        .mt-1 { margin-top: 5px; }
        .mt-2 { margin-top: 10px; }
        .mt-3 { margin-top: 15px; }
        .mt-4 { margin-top: 20px; }
        .mb-1 { margin-bottom: 5px; }
        .mb-2 { margin-bottom: 10px; }
        .mb-3 { margin-bottom: 15px; }
        .mb-4 { margin-bottom: 20px; }
        .ml-1 { margin-left: 5px; }
        .ml-2 { margin-left: 10px; }
        .ml-3 { margin-left: 15px; }
        .mr-1 { margin-right: 5px; }
        .mr-2 { margin-right: 10px; }
        .mr-3 { margin-right: 15px; }

        /* BOTÕES DE ÍCONE */
        .btn-icon {
            background: none;
            border: none;
            cursor: pointer;
            padding: 5px;
            margin: 0 3px;
            color: #666;
            transition: color 0.3s;
        }

        .btn-icon:hover {
            color: var(--secondary);
        }

        /* MODAL DE CONVÊNIO */
        .convenio-modal {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.7);
            display: flex;
            align-items: center;
            justify-content: center;
            z-index: 2000;
        }

        .convenio-modal-content {
            background: white;
            border-radius: 10px;
            padding: 30px;
            width: 90%;
            max-width: 500px;
            max-height: 80vh;
            overflow-y: auto;
        }

        /* ACESSO RESTRITO */
        .restricted-access {
            text-align: center;
            padding: 50px 20px;
            color: #666;
        }

        .restricted-access i {
            font-size: 4rem;
            color: var(--danger);
            margin-bottom: 20px;
        }

        /* OVERLAY DE SESSÃO EXPIRADA */
        .block-overlay {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.9);
            display: flex;
            align-items: center;
            justify-content: center;
            z-index: 3000;
        }
    </style>
</head>
<body>
    <!-- Tela de Carregamento Inicial -->
    <div id="loading-screen" class="loading-screen">
        <div class="loading-spinner"></div>
        <h2>Carregando Sistema...</h2>
        <p>Aguarde enquanto inicializamos o sistema</p>
    </div>

    <!-- Modal de Login -->
    <div id="login-modal" class="login-modal">
        <div class="login-container">
            <div class="login-logo">
                <i class="fas fa-chart-bar"></i>
                <h2>DataManager Pro</h2>
                <p>Sistema de Gerenciamento de Dados</p>
            </div>
            
            <div id="login-alert" class="login-alert error">
                <i class="fas fa-exclamation-circle"></i>
                <span>Usuário ou senha incorretos. Tente novamente.</span>
            </div>
            
            <form id="login-form">
                <div class="form-group">
                    <label class="form-label" for="login-email">
                        <i class="fas fa-envelope"></i> Email
                    </label>
                    <input type="email" class="form-control" id="login-email" 
                           placeholder="seu@email.com" required>
                </div>
                
                <div class="form-group">
                    <label class="form-label" for="login-password">
                        <i class="fas fa-lock"></i> Senha
                    </label>
                    <input type="password" class="form-control" id="login-password" 
                           placeholder="Digite sua senha" required>
                </div>
                
                <div class="form-group">
                    <button type="submit" class="btn btn-primary btn-block">
                        <i class="fas fa-sign-in-alt"></i> Entrar no Sistema
                    </button>
                </div>
                
                <div class="text-center mt-3">
                    <small>Versão 2.1.0 | Sistema de Gestão Completo</small>
                </div>
            </form>
            
            <div class="text-center mt-4">
                <div style="margin-bottom: 15px; color: #666;">
                    <strong>Credenciais de teste:</strong>
                </div>
                <div style="background: #f8f9fa; padding: 10px; border-radius: 5px; font-size: 0.9rem;">
                    <div><strong>Administrador:</strong> admin@datamanager.com / admin123</div>
                    <div><strong>Gerente:</strong> joao@empresa.com / senha123</div>
                    <div><strong>Analista:</strong> maria@empresa.com / senha123</div>
                </div>
            </div>
        </div>
    </div>

    <!-- Sistema Principal (inicialmente oculto) -->
    <div id="main-system" style="display: none;">
        <div class="container">
            <!-- Sidebar -->
            <aside class="sidebar">
                <div class="sidebar-header">
                    <div class="logo">
                        <i class="fas fa-chart-bar"></i>
                        <span>DataManager</span>
                    </div>
                </div>
                <ul class="nav-menu">
                    <li class="nav-item">
                        <a href="#" class="nav-link active" data-page="dashboard">
                            <i class="fas fa-tachometer-alt"></i>
                            <span class="nav-text">Dashboard</span>
                        </a>
                    </li>
                    <li class="nav-item">
                        <a href="#" class="nav-link" data-page="fechamento-diario">
                            <i class="fas fa-calendar-day"></i>
                            <span class="nav-text">Fechamento Diário</span>
                        </a>
                    </li>
                    <li class="nav-item">
                        <a href="#" class="nav-link" data-page="convenios">
                            <i class="fas fa-handshake"></i>
                            <span class="nav-text">Convênios</span>
                        </a>
                    </li>
                    <li class="nav-item">
                        <a href="#" class="nav-link" data-page="lancamentos">
                            <i class="fas fa-file-import"></i>
                            <span class="nav-text">Lançamentos</span>
                        </a>
                    </li>
                    <li class="nav-item">
                        <a href="#" class="nav-link" data-page="centro-custo">
                            <i class="fas fa-tags"></i>
                            <span class="nav-text">Centros de Custo</span>
                        </a>
                    </li>
                    <li class="nav-item">
                        <a href="#" class="nav-link" data-page="upload-excel">
                            <i class="fas fa-file-excel"></i>
                            <span class="nav-text">Importar Excel</span>
                        </a>
                    </li>
                    <li class="nav-item">
                        <a href="#" class="nav-link" data-page="relatorios">
                            <i class="fas fa-chart-pie"></i>
                            <span class="nav-text">Relatórios</span>
                        </a>
                    </li>
                    <li class="nav-item">
                        <a href="#" class="nav-link" data-page="usuarios">
                            <i class="fas fa-users"></i>
                            <span class="nav-text">Usuários</span>
                        </a>
                    </li>
                    <li class="nav-item">
                        <a href="#" class="nav-link" data-page="configuracoes">
                            <i class="fas fa-cog"></i>
                            <span class="nav-text">Configurações</span>
                        </a>
                    </li>
                </ul>
            </aside>

            <!-- Conteúdo Principal -->
            <main class="main-content">
                <!-- Header -->
                <div class="header">
                    <h1 id="page-title">Dashboard</h1>
                    <div class="user-info">
                        <div class="user-avatar" id="user-avatar">AD</div>
                        <div>
                            <div class="user-name" id="user-name">Carregando...</div>
                            <div class="user-role" id="user-role"></div>
                            <div id="user-centros" class="access-badge"></div>
                        </div>
                        <a href="#" id="logout-btn" class="btn btn-primary ml-3">
                            <i class="fas fa-sign-out-alt"></i> Sair
                        </a>
                    </div>
                </div>

                <!-- Páginas do Sistema -->
                <div id="dashboard" class="page active">
                    <!-- Conteúdo será carregado dinamicamente -->
                </div>

                <div id="fechamento-diario" class="page">
                    <!-- Conteúdo será carregado dinamicamente -->
                </div>

                <div id="convenios" class="page">
                    <!-- Conteúdo será carregado dinamicamente -->
                </div>

                <div id="lancamentos" class="page">
                    <!-- Conteúdo será carregado dinamicamente -->
                </div>

                <div id="centro-custo" class="page">
                    <!-- Conteúdo será carregado dinamicamente -->
                </div>

                <div id="upload-excel" class="page">
                    <!-- Conteúdo será carregado dinamicamente -->
                </div>

                <div id="relatorios" class="page">
                    <!-- Conteúdo será carregado dinamicamente -->
                </div>

                <div id="usuarios" class="page">
                    <!-- Conteúdo será carregado dinamicamente -->
                </div>

                <div id="configuracoes" class="page">
                    <!-- Conteúdo será carregado dinamicamente -->
                </div>
            </main>
        </div>
    </div>

    <!-- Modal para Cadastro de Convênio -->
    <div id="convenio-modal" class="convenio-modal" style="display: none;">
        <div class="convenio-modal-content">
            <h3 class="form-title">Cadastrar Novo Convênio</h3>
            <form id="convenio-form">
                <div class="form-group">
                    <label class="form-label" for="convenio-nome">Nome do Convênio *</label>
                    <input type="text" class="form-control" id="convenio-nome" required>
                </div>
                
                <div class="form-group">
                    <label class="form-label" for="convenio-documento">CNPJ/CPF *</label>
                    <input type="text" class="form-control" id="convenio-documento" 
                           placeholder="00.000.000/0000-00 ou 000.000.000-00" required>
                </div>
                
                <div class="form-group">
                    <label class="form-label" for="convenio-tipo">Tipo de Documento</label>
                    <select class="form-control" id="convenio-tipo" required>
                        <option value="CNPJ">CNPJ</option>
                        <option value="CPF">CPF</option>
                    </select>
                </div>
                
                <div class="form-group">
                    <label class="form-label" for="convenio-centro-custo">Centro de Custo Vinculado</label>
                    <select class="form-control" id="convenio-centro-custo">
                        <option value="">Nenhum</option>
                        <!-- Opções serão carregadas via JS -->
                    </select>
                </div>
                
                <div class="form-group">
                    <label class="form-label" for="convenio-status">Status</label>
                    <select class="form-control" id="convenio-status" required>
                        <option value="Ativo">Ativo</option>
                        <option value="Inativo">Inativo</option>
                    </select>
                </div>
                
                <div class="d-flex justify-between mt-3">
                    <button type="button" id="cancel-convenio" class="btn">Cancelar</button>
                    <button type="submit" class="btn btn-primary">Salvar Convênio</button>
                </div>
            </form>
        </div>
    </div>

    <script>
        // ============== BANCO DE DADOS COMPLETO ==============
        const database = {
            users: [
                { 
                    id: 1, 
                    name: "Administrador", 
                    email: "admin@datamanager.com", 
                    password: "admin123", 
                    role: "Administrador", 
                    centrosCusto: ["ADM", "VENDAS", "PROD", "TI", "RH", "MARK"],
                    lastLogin: new Date().toISOString(), 
                    status: "Ativo" 
                },
                { 
                    id: 2, 
                    name: "João Silva", 
                    email: "joao@empresa.com", 
                    password: "senha123", 
                    role: "Gerente", 
                    centrosCusto: ["VENDAS", "MARK"],
                    lastLogin: "2026-01-21T10:30:00", 
                    status: "Ativo" 
                },
                { 
                    id: 3, 
                    name: "Maria Santos", 
                    email: "maria@empresa.com", 
                    password: "senha123", 
                    role: "Analista", 
                    centrosCusto: ["VENDAS"],
                    lastLogin: "2026-01-20T14:15:00", 
                    status: "Ativo" 
                }
            ],
            
            fechamentosDiarios: [
                {
                    id: 1,
                    data: "2026-01-22",
                    responsavel: "João Silva",
                    centroCusto: "VENDAS",
                    dinheiro: 9220.00,
                    pix: 2450.00,
                    pixAjuste: 0.00,
                    lcm: 665.00,
                    amatur: 420.00,
                    pixMatriz: 0.00,
                    outrosFaturamentos: 0.00,
                    faturamentoL90Dinheiro: 0.00,
                    faturamentoL90Pix: 0.00,
                    despesas: 0.00,
                    valeAdiantamento: 0.00,
                    taxasBancarias: [
                        {
                            bancoId: 1,
                            bancoNome: "Banco do Brasil",
                            valor: 150.00
                        },
                        {
                            bancoId: 2,
                            bancoNome: "Itaú",
                            valor: 75.00
                        }
                    ],
                    convenios: [
                        { convenioId: 1, valor: 1500.00, nome: "Convênio Saúde" }
                    ],
                    totalFaturamentos: 12755.00,
                    totalDespesas: 225.00,
                    totalDiaria: 12530.00,
                    totalLiquidoDinheiro: 12530.00,
                    status: "Fechado",
                    createdAt: "2026-01-22T18:30:00",
                    createdBy: 2
                }
            ],
            
            convenios: [
                {
                    id: 1,
                    nome: "Convênio Saúde",
                    documento: "12.345.678/0001-90",
                    tipo: "CNPJ",
                    centroCusto: "VENDAS",
                    status: "Ativo",
                    createdAt: "2026-01-15T10:00:00",
                    createdBy: 1
                },
                {
                    id: 2,
                    nome: "Convênio Educação",
                    documento: "98.765.432/0001-10",
                    tipo: "CNPJ",
                    centroCusto: "MARK",
                    status: "Ativo",
                    createdAt: "2026-01-16T14:30:00",
                    createdBy: 1
                },
                {
                    id: 3,
                    nome: "Convênio Individual",
                    documento: "123.456.789-00",
                    tipo: "CPF",
                    centroCusto: "",
                    status: "Ativo",
                    createdAt: "2026-01-17T09:15:00",
                    createdBy: 2
                }
            ],
            
            centrosCusto: [
                { codigo: "ADM", nome: "Administrativo", responsavel: "João Silva", orcamento: 50000, gasto: 32500, status: "Ativo" },
                { codigo: "VENDAS", nome: "Vendas", responsavel: "Maria Santos", orcamento: 80000, gasto: 45200, status: "Ativo" },
                { codigo: "PROD", nome: "Produção", responsavel: "Carlos Oliveira", orcamento: 120000, gasto: 98750, status: "Ativo" },
                { codigo: "TI", nome: "Tecnologia da Informação", responsavel: "Ana Costa", orcamento: 75000, gasto: 42000, status: "Ativo" },
                { codigo: "RH", nome: "Recursos Humanos", responsavel: "Pedro Almeida", orcamento: 45000, gasto: 28000, status: "Ativo" },
                { codigo: "MARK", nome: "Marketing", responsavel: "Beatriz Lima", orcamento: 60000, gasto: 38500, status: "Ativo" }
            ],
            
            lancamentos: [
                { id: 1, data: "2026-01-22", descricao: "Compra de materiais", centroCusto: "ADM", valor: 1250.50, categoria: "Despesa", createdBy: 1 }
            ],
            
            bancos: [
                {
                    id: 1,
                    nome: "Banco do Brasil",
                    agencia: "1234-5",
                    conta: "98765-4",
                    status: "Ativo",
                    createdAt: "2026-01-15T10:00:00",
                    createdBy: 1
                },
                {
                    id: 2,
                    nome: "Itaú",
                    agencia: "5678-9",
                    conta: "54321-0",
                    status: "Ativo",
                    createdAt: "2026-01-16T14:30:00",
                    createdBy: 1
                },
                {
                    id: 3,
                    nome: "Bradesco",
                    agencia: "9012-3",
                    conta: "13579-2",
                    status: "Ativo",
                    createdAt: "2026-01-17T09:15:00",
                    createdBy: 2
                }
            ],
            
            settings: {
                systemName: "DataManager Pro",
                currency: "BRL",
                dateFormat: "dd/mm/yyyy",
                sessionTimeout: 30,
                emailNotifications: true,
                backupAutomatico: true
            }
        };

        // ============== ESTADO DA APLICAÇÃO ==============
        let currentUser = null;
        let currentPage = "dashboard";
        let sessionTimer = null;
        let isSystemLocked = true;
        let charts = {};
        let selectedConvenios = [];
        let selectedTaxasBancarias = [];

        // ============== INICIALIZAÇÃO ==============
        document.addEventListener('DOMContentLoaded', function() {
            setTimeout(() => {
                document.getElementById('loading-screen').style.display = 'none';
                showLoginModal();
            }, 1500);
            
            setupLogin();
            
            document.getElementById('logout-btn').addEventListener('click', function(e) {
                e.preventDefault();
                logoutUser();
            });
            
            // Configurar modal de convênio
            document.getElementById('cancel-convenio').addEventListener('click', function() {
                document.getElementById('convenio-modal').style.display = 'none';
            });
            
            document.getElementById('convenio-form').addEventListener('submit', function(e) {
                e.preventDefault();
                saveConvenio();
            });
        });

        // ============== SISTEMA DE LOGIN ==============
        function setupLogin() {
            const loginForm = document.getElementById('login-form');
            const loginAlert = document.getElementById('login-alert');
            
            loginForm.addEventListener('submit', function(e) {
                e.preventDefault();
                
                const email = document.getElementById('login-email').value.trim();
                const password = document.getElementById('login-password').value;
                
                if (!email || !password) {
                    showLoginError("Por favor, preencha todos os campos.");
                    return;
                }
                
                const user = database.users.find(u => 
                    u.email === email && u.password === password && u.status === "Ativo"
                );
                
                if (user) {
                    user.lastLogin = new Date().toISOString();
                    loginUser(user);
                } else {
                    showLoginError("Credenciais inválidas ou usuário inativo.");
                }
            });
            
            ['login-email', 'login-password'].forEach(id => {
                document.getElementById(id).addEventListener('input', function() {
                    loginAlert.style.display = 'none';
                });
            });
        }
        
        function showLoginError(message) {
            const loginAlert = document.getElementById('login-alert');
            loginAlert.querySelector('span').textContent = message;
            loginAlert.style.display = 'block';
            loginAlert.style.animation = 'none';
            setTimeout(() => {
                loginAlert.style.animation = 'fadeIn 0.3s';
            }, 10);
        }
        
        function loginUser(user) {
            currentUser = user;
            isSystemLocked = false;
            
            document.getElementById('login-modal').style.display = 'none';
            document.getElementById('main-system').style.display = 'flex';
            
            updateUserInfo();
            startSessionTimer();
            loadInitialContent();
            
            showAlert(`Bem-vindo(a), ${user.name}!`, 'success');
            console.log(`Usuário ${user.name} (${user.role}) fez login.`);
        }
        
        function logoutUser() {
            if (confirm('Tem certeza que deseja sair do sistema?')) {
                if (sessionTimer) {
                    clearTimeout(sessionTimer);
                    sessionTimer = null;
                }
                
                currentUser = null;
                isSystemLocked = true;
                
                document.getElementById('main-system').style.display = 'none';
                showLoginModal();
                document.getElementById('login-form').reset();
                document.getElementById('login-alert').style.display = 'none';
                
                showAlert('Você saiu do sistema com sucesso.', 'info');
            }
        }
        
        function showLoginModal() {
            document.getElementById('login-modal').style.display = 'flex';
        }
        
        function updateUserInfo() {
            if (!currentUser) return;
            
            document.getElementById('user-name').textContent = currentUser.name;
            document.getElementById('user-role').textContent = currentUser.role;
            document.getElementById('user-avatar').textContent = 
                currentUser.name.split(' ').map(n => n[0]).join('').substring(0, 2);
            
            const centrosBadge = document.getElementById('user-centros');
            if (currentUser.role === "Administrador") {
                centrosBadge.textContent = "Acesso Total";
                centrosBadge.style.backgroundColor = '#d4edda';
                centrosBadge.style.color = '#155724';
            } else if (currentUser.centrosCusto && currentUser.centrosCusto.length > 0) {
                centrosBadge.textContent = `${currentUser.centrosCusto.length} centro(s)`;
            } else {
                centrosBadge.textContent = "Sem centros designados";
                centrosBadge.style.backgroundColor = '#f8d7da';
                centrosBadge.style.color = '#721c24';
            }
        }
        
        function startSessionTimer() {
            const timeoutMinutes = database.settings.sessionTimeout || 30;
            const timeoutMs = timeoutMinutes * 60 * 1000;
            
            if (sessionTimer) {
                clearTimeout(sessionTimer);
            }
            
            sessionTimer = setTimeout(() => {
                showSessionExpired();
            }, timeoutMs);
            
            ['click', 'mousemove', 'keypress'].forEach(event => {
                document.addEventListener(event, () => {
                    if (sessionTimer) {
                        clearTimeout(sessionTimer);
                        sessionTimer = setTimeout(() => {
                            showSessionExpired();
                        }, timeoutMs);
                    }
                });
            });
        }
        
        function showSessionExpired() {
            const overlay = document.createElement('div');
            overlay.className = 'block-overlay';
            overlay.innerHTML = `
                <div style="background: white; padding: 30px; border-radius: 10px; color: #333; max-width: 500px;">
                    <i class="fas fa-clock" style="font-size: 3rem; color: var(--warning); margin-bottom: 20px;"></i>
                    <h2>Sessão Expirada</h2>
                    <p>Sua sessão expirou por inatividade. Por favor, faça login novamente.</p>
                    <button id="reload-session" class="btn btn-primary" style="margin-top: 20px;">
                        <i class="fas fa-redo"></i> Fazer Login Novamente
                    </button>
                </div>
            `;
            
            document.body.appendChild(overlay);
            
            document.getElementById('reload-session').addEventListener('click', function() {
                document.body.removeChild(overlay);
                logoutUser();
            });
            
            setTimeout(() => {
                if (overlay.parentNode) {
                    document.body.removeChild(overlay);
                    logoutUser();
                }
            }, 5000);
        }

        // ============== CONTROLE DE ACESSO ==============
        function checkAccess(page) {
            if (!currentUser || isSystemLocked) {
                showLoginModal();
                return false;
            }
            
            if (page === 'usuarios' && currentUser.role !== 'Administrador') {
                showAlert('Acesso restrito. Apenas administradores podem gerenciar usuários.', 'warning');
                return false;
            }
            
            return true;
        }
        
        function showAlert(message, type = 'info') {
            const existingAlerts = document.querySelectorAll('.alert');
            existingAlerts.forEach(alert => {
                if (alert.parentNode) {
                    alert.parentNode.removeChild(alert);
                }
            });
            
            const alert = document.createElement('div');
            alert.className = `alert alert-${type}`;
            alert.innerHTML = `
                <i class="fas ${type === 'success' ? 'fa-check-circle' : 
                               type === 'warning' ? 'fa-exclamation-triangle' : 
                               type === 'danger' ? 'fa-times-circle' : 'fa-info-circle'}"></i>
                ${message}
                <button class="btn-icon close-alert" style="float: right; color: inherit;">
                    <i class="fas fa-times"></i>
                </button>
            `;
            
            const mainContent = document.querySelector('.main-content');
            if (mainContent) {
                mainContent.insertBefore(alert, mainContent.firstChild);
                
                alert.querySelector('.close-alert').addEventListener('click', function() {
                    alert.remove();
                });
                
                setTimeout(() => {
                    if (alert.parentNode) {
                        alert.remove();
                    }
                }, 5000);
            }
        }

        // ============== NAVEGAÇÃO ==============
        function setupNavigation() {
            document.querySelectorAll('.nav-link').forEach(link => {
                link.addEventListener('click', function(e) {
                    e.preventDefault();
                    
                    const pageId = this.getAttribute('data-page');
                    if (!checkAccess(pageId)) {
                        return;
                    }
                    
                    document.querySelectorAll('.nav-link').forEach(item => {
                        item.classList.remove('active');
                    });
                    this.classList.add('active');
                    
                    showPage(pageId);
                    
                    document.getElementById('page-title').textContent = 
                        this.querySelector('.nav-text').textContent;
                });
            });
        }
        
        function showPage(pageId) {
            if (!checkAccess(pageId)) return;
            
            document.querySelectorAll('.page').forEach(page => {
                page.classList.remove('active');
            });
            
            const pageElement = document.getElementById(pageId);
            if (pageElement) {
                pageElement.classList.add('active');
                currentPage = pageId;
                
                loadPageContent(pageId);
            }
        }

        // ============== CARREGAMENTO DE CONTEÚDO ==============
        function loadInitialContent() {
            setupNavigation();
            loadPageContent('dashboard');
            setupGlobalEvents();
            setupRestrictedAccessElements();
        }
        
        function loadPageContent(pageId) {
            switch(pageId) {
                case 'dashboard':
                    loadDashboardContent();
                    break;
                case 'fechamento-diario':
                    loadFechamentoDiarioContent();
                    break;
                case 'convenios':
                    loadConveniosContent();
                    break;
                case 'lancamentos':
                    loadLancamentosContent();
                    break;
                case 'centro-custo':
                    loadCentroCustoContent();
                    break;
                case 'upload-excel':
                    loadUploadExcelContent();
                    break;
                case 'relatorios':
                    loadRelatoriosContent();
                    break;
                case 'usuarios':
                    loadUsuariosContent();
                    break;
                case 'configuracoes':
                    loadConfiguracoesContent();
                    break;
            }
        }

        // ============== DASHBOARD ==============
        function loadDashboardContent() {
            const dashboardPage = document.getElementById('dashboard');
            
            dashboardPage.innerHTML = `
                <div class="dashboard-welcome">
                    <h2>Bem-vindo ao DataManager Pro</h2>
                    <p>Olá, ${currentUser ? currentUser.name : 'Usuário'}! Aqui está o resumo do seu sistema.</p>
                </div>
                
                <div class="cards-container">
                    <div class="card">
                        <div class="card-title">Fechamentos do Mês</div>
                        <div class="card-value" id="total-fechamentos">0</div>
                        <div class="card-footer">
                            <span class="success-text" id="fechamentos-variacao">-</span> vs mês anterior
                        </div>
                    </div>
                    <div class="card">
                        <div class="card-title">Total em Convênios</div>
                        <div class="card-value" id="total-convenios">R$ 0,00</div>
                        <div class="card-footer">
                            <span class="success-text" id="convenios-variacao">-</span> ativos
                        </div>
                    </div>
                    <div class="card">
                        <div class="card-title">Centros de Custo</div>
                        <div class="card-value" id="total-centros">0</div>
                        <div class="card-footer">
                            <span class="warning-text"><i class="fas fa-arrow-down"></i> 0</span> inativos
                        </div>
                    </div>
                    <div class="card">
                        <div class="card-title">Usuários Ativos</div>
                        <div class="card-value" id="total-usuarios">0</div>
                        <div class="card-footer">
                            <span class="success-text"><i class="fas fa-arrow-up"></i> 0</span> este mês
                        </div>
                    </div>
                </div>

                <div class="dashboard-container">
                    <div>
                        <div class="chart-container">
                            <div class="chart-header">
                                <div class="chart-title">Faturamento por Tipo</div>
                            </div>
                            <canvas id="faturamentoChart"></canvas>
                        </div>
                        
                        <div class="chart-container">
                            <div class="chart-header">
                                <div class="chart-title">Convênios Ativos</div>
                            </div>
                            <canvas id="conveniosChart"></canvas>
                        </div>
                    </div>
                    
                    <div>
                        <div class="fechamento-summary">
                            <div class="chart-title">Últimos Fechamentos</div>
                            <div id="ultimos-fechamentos">
                                <!-- Últimos fechamentos serão carregados aqui -->
                            </div>
                            <div class="text-center mt-3">
                                <button class="btn btn-primary" id="ver-todos-fechamentos">
                                    <i class="fas fa-list"></i> Ver Todos
                                </button>
                            </div>
                        </div>
                        
                        <div class="fechamento-summary mt-3">
                            <div class="chart-title">Convênios Recentes</div>
                            <div id="ultimos-convenios">
                                <!-- Últimos convênios serão carregados aqui -->
                            </div>
                            <div class="text-center mt-3">
                                <button class="btn btn-primary" id="ver-todos-convenios">
                                    <i class="fas fa-handshake"></i> Ver Todos
                                </button>
                            </div>
                        </div>
                    </div>
                </div>
            `;
            
            // Configurar eventos
            document.getElementById('ver-todos-fechamentos').addEventListener('click', function() {
                showPage('fechamento-diario');
            });
            
            document.getElementById('ver-todos-convenios').addEventListener('click', function() {
                showPage('convenios');
            });
            
            // Inicializar gráficos e dados
            setupDashboardCharts();
            updateDashboardData();
        }
        
        function setupDashboardCharts() {
            // Gráfico de faturamento
            const faturamentoCtx = document.getElementById('faturamentoChart');
            if (faturamentoCtx) {
                charts.faturamentoChart = new Chart(faturamentoCtx.getContext('2d'), {
                    type: 'doughnut',
                    data: {
                        labels: ['Dinheiro', 'PIX', 'Convênios', 'Outros'],
                        datasets: [{
                            data: [55, 25, 15, 5],
                            backgroundColor: [
                                'rgba(39, 174, 96, 0.8)',
                                'rgba(52, 152, 219, 0.8)',
                                'rgba(155, 89, 182, 0.8)',
                                'rgba(241, 196, 15, 0.8)'
                            ],
                            borderWidth: 1
                        }]
                    },
                    options: {
                        responsive: true,
                        plugins: {
                            legend: {
                                position: 'bottom',
                            }
                        }
                    }
                });
            }
            
            // Gráfico de convênios
            const conveniosCtx = document.getElementById('conveniosChart');
            if (conveniosCtx) {
                charts.conveniosChart = new Chart(conveniosCtx.getContext('2d'), {
                    type: 'bar',
                    data: {
                        labels: ['Convênio Saúde', 'Convênio Educação', 'Convênio Individual'],
                        datasets: [{
                            label: 'Valor (R$)',
                            data: [1500, 2000, 800],
                            backgroundColor: 'rgba(155, 89, 182, 0.7)',
                            borderColor: 'rgba(155, 89, 182, 1)',
                            borderWidth: 1
                        }]
                    },
                    options: {
                        responsive: true,
                        scales: {
                            y: {
                                beginAtZero: true,
                                ticks: {
                                    callback: function(value) {
                                        return 'R$ ' + value.toLocaleString('pt-BR');
                                    }
                                }
                            }
                        }
                    }
                });
            }
        }
        
        function updateDashboardData() {
            if (!currentUser) return;
            
            // Calcular total de taxas bancárias do mês
            const hoje = new Date();
            const mesAtual = hoje.getMonth() + 1;
            const anoAtual = hoje.getFullYear();
            
            const fechamentosMesAtual = database.fechamentosDiarios.filter(f => {
                const data = new Date(f.data);
                return data.getMonth() + 1 === mesAtual && data.getFullYear() === anoAtual;
            });
            
            const totalTaxasBancarias = fechamentosMesAtual.reduce((total, fechamento) => {
                return total + (fechamento.taxasBancarias ? fechamento.taxasBancarias.reduce((sum, t) => sum + t.valor, 0) : 0);
            }, 0);
            
            // Atualizar cards
            document.getElementById('total-fechamentos').textContent = fechamentosMesAtual.length;
            
            // Total em convênios
            const totalConvenios = fechamentosMesAtual.reduce((total, fechamento) => {
                return total + (fechamento.convenios ? fechamento.convenios.reduce((sum, c) => sum + c.valor, 0) : 0);
            }, 0);
            document.getElementById('total-convenios').textContent = formatCurrency(totalConvenios);
            
            // Atualizar rodapé do card de convênios
            const cardConvenios = document.querySelector('.card:nth-child(2) .card-footer');
            if (cardConvenios) {
                const ativosText = database.convenios.filter(c => c.status === 'Ativo').length;
                cardConvenios.innerHTML = `<span class="success-text">${ativosText}</span> ativos | <span class="danger-text">Taxas: ${formatCurrency(totalTaxasBancarias)}</span>`;
            }
            
            // Centros de custo
            const centrosAtivos = database.centrosCusto.filter(c => c.status === 'Ativo').length;
            const centrosInativos = database.centrosCusto.filter(c => c.status === 'Inativo').length;
            document.getElementById('total-centros').textContent = database.centrosCusto.length;
            
            // Usuários ativos
            const usuariosAtivos = database.users.filter(u => u.status === 'Ativo').length;
            document.getElementById('total-usuarios').textContent = usuariosAtivos;
            
            // Últimos fechamentos
            const ultimosFechamentos = database.fechamentosDiarios
                .sort((a, b) => new Date(b.data) - new Date(a.data))
                .slice(0, 3);
            
            const fechamentosHTML = ultimosFechamentos.map(f => `
                <div class="summary-item">
                    <span class="summary-label">${formatDate(f.data)}</span>
                    <span class="summary-value">${formatCurrency(f.totalDiaria)}</span>
                </div>
            `).join('');
            
            document.getElementById('ultimos-fechamentos').innerHTML = fechamentosHTML || 
                '<div class="text-center">Nenhum fechamento encontrado</div>';
            
            // Últimos convênios
            const ultimosConvenios = database.convenios
                .filter(c => c.status === 'Ativo')
                .slice(0, 3);
            
            const conveniosHTML = ultimosConvenios.map(c => `
                <div class="summary-item">
                    <span class="summary-label">${c.nome}</span>
                    <span class="summary-value">${c.documento}</span>
                </div>
            `).join('');
            
            document.getElementById('ultimos-convenios').innerHTML = conveniosHTML || 
                '<div class="text-center">Nenhum convênio encontrado</div>';
        }

        // ============== FECHAMENTO DIÁRIO COM CONVÊNIOS ==============
        function loadFechamentoDiarioContent() {
            const page = document.getElementById('fechamento-diario');
            
            page.innerHTML = `
                <div class="d-flex justify-between mb-3">
                    <h2>Fechamento Diário</h2>
                    <button class="btn btn-success" id="add-fechamento">
                        <i class="fas fa-plus"></i> Novo Fechamento
                    </button>
                </div>
                
                <!-- Filtros -->
                <div class="form-container mb-3">
                    <h4>Filtros</h4>
                    <div class="fechamento-form">
                        <div class="fechamento-group">
                            <label class="fechamento-label">Mês/Ano</label>
                            <input type="month" id="filtro-mes" class="fechamento-input" value="${new Date().toISOString().substring(0, 7)}">
                        </div>
                        <div class="fechamento-group">
                            <label class="fechamento-label">&nbsp;</label>
                            <button class="btn btn-primary" id="aplicar-filtro">
                                <i class="fas fa-filter"></i> Filtrar
                            </button>
                        </div>
                    </div>
                </div>
                
                <!-- Tabela de Fechamentos -->
                <div class="table-container">
                    <table class="table">
                        <thead>
                            <tr>
                                <th>Data</th>
                                <th>Responsável</th>
                                <th>Dinheiro</th>
                                <th>PIX</th>
                                <th>Convênios</th>
                                <th>Total</th>
                                <th>Ações</th>
                            </tr>
                        </thead>
                        <tbody id="fechamentos-table">
                            <!-- Dados serão carregados via JavaScript -->
                        </tbody>
                    </table>
                </div>
                
                <!-- Formulário de Fechamento -->
                <div id="fechamento-form-container" style="display: none;">
                    <div class="form-container">
                        <h3 class="form-title">Novo Fechamento Diário</h3>
                        
                        <div class="tabs-container">
                            <div class="tabs">
                                <button class="tab active" data-tab="basico">Informações Básicas</button>
                                <button class="tab" data-tab="convenios">Convênios</button>
                                <button class="tab" data-tab="taxas-bancarias">Taxas Bancárias</button>
                                <button class="tab" data-tab="resumo">Resumo</button>
                            </div>
                            
                            <!-- Tab 1: Informações Básicas -->
                            <div class="tab-content active" id="tab-basico">
                                <form id="fechamento-form-basico">
                                    <div class="fechamento-form">
                                        <div class="fechamento-group">
                                            <label class="fechamento-label" for="fechamento-data">Data *</label>
                                            <input type="date" class="fechamento-input" id="fechamento-data" required>
                                        </div>
                                        
                                        <div class="fechamento-group">
                                            <label class="fechamento-label" for="fechamento-responsavel">Responsável *</label>
                                            <input type="text" class="fechamento-input" id="fechamento-responsavel" required>
                                        </div>
                                        
                                        <div class="fechamento-group">
                                            <label class="fechamento-label" for="fechamento-centro-custo">Centro de Custo</label>
                                            <select class="fechamento-input" id="fechamento-centro-custo">
                                                <option value="">Selecione...</option>
                                                ${getCentrosCustoOptions()}
                                            </select>
                                        </div>
                                    </div>
                                    
                                    <div class="fechamento-form mt-3">
                                        <div class="fechamento-group">
                                            <label class="fechamento-label" for="fechamento-dinheiro">Dinheiro (R$)</label>
                                            <input type="number" step="0.01" class="fechamento-input" id="fechamento-dinheiro" value="0.00">
                                        </div>
                                        
                                        <div class="fechamento-group">
                                            <label class="fechamento-label" for="fechamento-pix">PIX (R$)</label>
                                            <input type="number" step="0.01" class="fechamento-input" id="fechamento-pix" value="0.00">
                                        </div>
                                        
                                        <div class="fechamento-group">
                                            <label class="fechamento-label" for="fechamento-outros">Outros (R$)</label>
                                            <input type="number" step="0.01" class="fechamento-input" id="fechamento-outros" value="0.00">
                                        </div>
                                    </div>
                                    
                                    <div class="mt-3">
                                        <button type="button" class="btn btn-primary" id="proximo-convenios">
                                            Próximo: Convênios <i class="fas fa-arrow-right"></i>
                                        </button>
                                    </div>
                                </form>
                            </div>
                            
                            <!-- Tab 2: Convênios -->
                            <div class="tab-content" id="tab-convenios">
                                <div class="mb-3">
                                    <div class="d-flex justify-between">
                                        <h4>Selecionar Convênios</h4>
                                        <button type="button" class="btn btn-sm btn-primary" id="add-convenio-fechamento">
                                            <i class="fas fa-plus"></i> Novo Convênio
                                        </button>
                                    </div>
                                    <p class="text-muted">Selecione os convênios que fazem parte deste fechamento:</p>
                                </div>
                                
                                <div id="convenios-selecao">
                                    <!-- Convênios serão carregados aqui -->
                                </div>
                                
                                <div class="mt-3" id="convenios-selecionados-container" style="display: none;">
                                    <h5>Convênios Selecionados</h5>
                                    <div id="convenios-selecionados">
                                        <!-- Convênios selecionados aparecerão aqui -->
                                    </div>
                                    <div class="mt-2">
                                        <strong>Total Convênios:</strong> 
                                        <span id="total-convenios-selecionados">R$ 0,00</span>
                                    </div>
                                </div>
                                
                                <div class="mt-3">
                                    <button type="button" class="btn btn-secondary" id="voltar-basico">
                                        <i class="fas fa-arrow-left"></i> Voltar
                                    </button>
                                    <button type="button" class="btn btn-primary" id="proximo-resumo-convenios">
                                        Próximo: Taxas Bancárias <i class="fas fa-arrow-right"></i>
                                    </button>
                                </div>
                            </div>
                        
                            <!-- Tab 3: Taxas Bancárias -->
                            <div class="tab-content" id="tab-taxas-bancarias">
                                <div class="mb-3">
                                    <div class="d-flex justify-between">
                                        <h4>Taxas Bancárias</h4>
                                        <button type="button" class="btn btn-sm btn-primary" id="add-banco-fechamento">
                                            <i class="fas fa-university"></i> Novo Banco
                                        </button>
                                    </div>
                                    <p class="text-muted">Adicione as taxas bancárias deste fechamento:</p>
                                </div>
                                
                                <div id="bancos-selecao">
                                    <!-- Bancos serão carregados aqui -->
                                </div>
                                
                                <div class="mt-3" id="taxas-selecionadas-container" style="display: none;">
                                    <h5>Taxas Bancárias Selecionadas</h5>
                                    <div id="taxas-selecionadas">
                                        <!-- Taxas selecionadas aparecerão aqui -->
                                    </div>
                                    <div class="mt-2">
                                        <strong>Total Taxas Bancárias:</strong> 
                                        <span id="total-taxas-selecionadas">R$ 0,00</span>
                                    </div>
                                </div>
                                
                                <div class="mt-3">
                                    <button type="button" class="btn btn-secondary" id="voltar-convenios-taxas">
                                        <i class="fas fa-arrow-left"></i> Voltar para Convênios
                                    </button>
                                    <button type="button" class="btn btn-primary" id="proximo-resumo-taxas">
                                        Próximo: Resumo <i class="fas fa-arrow-right"></i>
                                    </button>
                                </div>
                            </div>
                            
                            <!-- Tab 4: Resumo -->
                            <div class="tab-content" id="tab-resumo">
                                <div class="totals-container">
                                    <div class="total-item">
                                        <span class="total-label">Dinheiro:</span>
                                        <span class="total-value" id="resumo-dinheiro">R$ 0,00</span>
                                    </div>
                                    <div class="total-item">
                                        <span class="total-label">PIX:</span>
                                        <span class="total-value" id="resumo-pix">R$ 0,00</span>
                                    </div>
                                    <div class="total-item">
                                        <span class="total-label">Outros:</span>
                                        <span class="total-value" id="resumo-outros">R$ 0,00</span>
                                    </div>
                                    <div class="total-item">
                                        <span class="total-label">Convênios:</span>
                                        <span class="total-value" id="resumo-convenios">R$ 0,00</span>
                                    </div>
                                    <div class="total-item" style="border-top: 2px solid #ddd;">
                                        <span class="total-label"><strong>Total Faturamentos:</strong></span>
                                        <span class="total-value"><strong id="resumo-total-faturamentos">R$ 0,00</strong></span>
                                    </div>
                                    <div class="total-item">
                                        <span class="total-label">Despesas Gerais:</span>
                                        <span class="total-value" id="resumo-despesas">R$ 0,00</span>
                                    </div>
                                    <div class="total-item">
                                        <span class="total-label">Taxas Bancárias:</span>
                                        <span class="total-value text-danger" id="resumo-taxas-bancarias">R$ 0,00</span>
                                    </div>
                                    <div class="total-item">
                                        <span class="total-label"><strong>Total Líquido:</strong></span>
                                        <span class="total-value"><strong id="resumo-total-liquido">R$ 0,00</strong></span>
                                    </div>
                                </div>
                                
                                <div class="mt-3">
                                    <button type="button" class="btn btn-secondary" id="voltar-taxas">
                                        <i class="fas fa-arrow-left"></i> Voltar para Taxas
                                    </button>
                                    <button type="submit" class="btn btn-success" id="salvar-fechamento">
                                        <i class="fas fa-save"></i> Salvar Fechamento
                                    </button>
                                </div>
                            </div>
                        </div>
                        
                        <div class="mt-3">
                            <button type="button" id="cancel-fechamento" class="btn btn-block">
                                Cancelar
                            </button>
                        </div>
                    </div>
                </div>
            `;
            
            setupFechamentoEvents();
            updateFechamentosTable();
        }
        
        // Modal para cadastro de banco
        function showBancoModal() {
            // Criar modal dinamicamente
            const modalHTML = `
                <div id="banco-modal" class="convenio-modal" style="display: flex;">
                    <div class="convenio-modal-content">
                        <h3 class="form-title">Cadastrar Novo Banco</h3>
                        <form id="banco-form">
                            <div class="form-group">
                                <label class="form-label" for="banco-nome">Nome do Banco *</label>
                                <input type="text" class="form-control" id="banco-nome" required>
                            </div>
                            
                            <div class="form-group">
                                <label class="form-label" for="banco-agencia">Agência *</label>
                                <input type="text" class="form-control" id="banco-agencia" 
                                       placeholder="Ex: 1234-5" required>
                            </div>
                            
                            <div class="form-group">
                                <label class="form-label" for="banco-conta">Conta Corrente *</label>
                                <input type="text" class="form-control" id="banco-conta" 
                                       placeholder="Ex: 98765-4" required>
                            </div>
                            
                            <div class="form-group">
                                <label class="form-label" for="banco-status">Status</label>
                                <select class="form-control" id="banco-status" required>
                                    <option value="Ativo">Ativo</option>
                                    <option value="Inativo">Inativo</option>
                                </select>
                            </div>
                            
                            <div class="d-flex justify-between mt-3">
                                <button type="button" id="cancel-banco" class="btn">Cancelar</button>
                                <button type="submit" class="btn btn-primary">Salvar Banco</button>
                            </div>
                        </form>
                    </div>
                </div>
            `;
            
            // Remover modal existente se houver
            const existingModal = document.getElementById('banco-modal');
            if (existingModal) {
                existingModal.remove();
            }
            
            // Adicionar modal ao body
            document.body.insertAdjacentHTML('beforeend', modalHTML);
            
            // Configurar eventos do modal
            document.getElementById('cancel-banco').addEventListener('click', function() {
                document.getElementById('banco-modal').remove();
            });
            
            document.getElementById('banco-form').addEventListener('submit', function(e) {
                e.preventDefault();
                saveBanco();
            });
        }

        // Salvar banco
        function saveBanco() {
            const nome = document.getElementById('banco-nome').value;
            const agencia = document.getElementById('banco-agencia').value;
            const conta = document.getElementById('banco-conta').value;
            const status = document.getElementById('banco-status').value;
            
            const novoBanco = {
                id: database.bancos.length + 1,
                nome: nome,
                agencia: agencia,
                conta: conta,
                status: status,
                createdAt: new Date().toISOString(),
                createdBy: currentUser.id
            };
            
            database.bancos.push(novoBanco);
            
            // Fechar modal
            document.getElementById('banco-modal').remove();
            
            // Recarregar seleção de bancos
            if (currentPage === 'fechamento-diario') {
                loadBancosForSelection();
            }
            
            showAlert('Banco cadastrado com sucesso!', 'success');
        }
        
        function setupFechamentoEvents() {
            // Novo fechamento
            document.getElementById('add-fechamento').addEventListener('click', function() {
                document.getElementById('fechamento-form-container').style.display = 'block';
                resetFechamentoForm();
                
                // Preencher valores padrão
                document.getElementById('fechamento-data').valueAsDate = new Date();
                document.getElementById('fechamento-responsavel').value = currentUser.name;
                
                // Carregar convênios
                loadConveniosForSelection();
                loadBancosForSelection();
                
                // Atualizar cálculo
                updateResumoFechamento();
            });
            
            // Cancelar fechamento
            document.getElementById('cancel-fechamento').addEventListener('click', function() {
                document.getElementById('fechamento-form-container').style.display = 'none';
                resetFechamentoForm();
            });
            
            // Aplicar filtro
            document.getElementById('aplicar-filtro').addEventListener('click', function() {
                updateFechamentosTable();
            });
            
            // Navegação entre tabs
            document.getElementById('proximo-convenios').addEventListener('click', function() {
                showTab('convenios');
            });
            
            document.getElementById('voltar-basico').addEventListener('click', function() {
                showTab('basico');
            });
            
            document.getElementById('proximo-resumo-convenios').addEventListener('click', function() {
                showTab('taxas-bancarias');
            });
            
            document.getElementById('voltar-convenios-taxas').addEventListener('click', function() {
                showTab('convenios');
            });
            
            document.getElementById('proximo-resumo-taxas').addEventListener('click', function() {
                showTab('resumo');
            });
            
            document.getElementById('voltar-taxas').addEventListener('click', function() {
                showTab('taxas-bancarias');
            });
            
            // Adicionar novo banco
            document.getElementById('add-banco-fechamento').addEventListener('click', function() {
                showBancoModal();
            });

            // Salvar fechamento
            document.getElementById('salvar-fechamento').addEventListener('click', function() {
                saveFechamento();
            });
            
            // Adicionar novo convênio
            document.getElementById('add-convenio-fechamento').addEventListener('click', function() {
                showConvenioModal();
            });
            
            // Atualizar cálculo quando valores mudarem
            ['fechamento-dinheiro', 'fechamento-pix', 'fechamento-outros'].forEach(id => {
                document.getElementById(id).addEventListener('input', updateResumoFechamento);
            });
        }
        
        function showTab(tabName) {
            // Esconder todas as tabs
            document.querySelectorAll('.tab-content').forEach(tab => {
                tab.classList.remove('active');
            });
            
            document.querySelectorAll('.tab').forEach(tab => {
                tab.classList.remove('active');
            });
            
            // Mostrar tab selecionada
            document.getElementById(`tab-${tabName}`).classList.add('active');
            document.querySelector(`.tab[data-tab="${tabName}"]`).classList.add('active');
        }
        
        function resetFechamentoForm() {
            // Resetar form básico
            document.getElementById('fechamento-form-basico').reset();
            
            // Resetar convênios selecionados
            selectedConvenios = [];
            selectedTaxasBancarias = [];
            
            document.getElementById('convenios-selecionados-container').style.display = 'none';
            document.getElementById('taxas-selecionadas-container').style.display = 'none';
            
            // Resetar tabs
            showTab('basico');
        }
        
        function loadConveniosForSelection() {
            const container = document.getElementById('convenios-selecao');
            if (!container) return;
            
            const conveniosAtivos = database.convenios.filter(c => c.status === 'Ativo');
            
            if (conveniosAtivos.length === 0) {
                container.innerHTML = `
                    <div class="alert alert-warning">
                        <i class="fas fa-info-circle"></i> Nenhum convênio ativo cadastrado.
                        <button class="btn btn-sm btn-primary ml-2" id="cadastrar-convenio-now">
                            <i class="fas fa-plus"></i> Cadastrar Primeiro Convênio
                        </button>
                    </div>
                `;
                
                document.getElementById('cadastrar-convenio-now').addEventListener('click', function() {
                    showConvenioModal();
                });
                return;
            }
            
            let html = '<div class="convenios-grid">';
            
            conveniosAtivos.forEach(convenio => {
                const isSelected = selectedConvenios.some(c => c.id === convenio.id);
                
                html += `
                    <div class="convenio-card ${isSelected ? 'selected' : ''}" data-id="${convenio.id}">
                        <div class="convenio-nome">${convenio.nome}</div>
                        <div class="convenio-documento">${convenio.documento}</div>
                        <div class="convenio-valor-input">
                            <input type="number" step="0.01" 
                                   class="convenio-valor-input-field" 
                                   data-id="${convenio.id}"
                                   placeholder="Valor (R$)"
                                   value="${isSelected ? selectedConvenios.find(c => c.id === convenio.id).valor || '' : ''}">
                        </div>
                    </div>
                `;
            });
            
            html += '</div>';
            container.innerHTML = html;
            
            // Configurar eventos dos cards
            document.querySelectorAll('.convenio-card').forEach(card => {
                card.addEventListener('click', function() {
                    const convenioId = parseInt(this.getAttribute('data-id'));
                    toggleConvenioSelection(convenioId);
                });
            });
            
            // Configurar eventos dos inputs de valor
            document.querySelectorAll('.convenio-valor-input-field').forEach(input => {
                input.addEventListener('click', function(e) {
                    e.stopPropagation();
                });
                
                input.addEventListener('input', function() {
                    const convenioId = parseInt(this.getAttribute('data-id'));
                    updateConvenioValor(convenioId, parseFloat(this.value) || 0);
                });
            });
        }
        
        function toggleConvenioSelection(convenioId) {
            const convenio = database.convenios.find(c => c.id === convenioId);
            if (!convenio) return;
            
            const index = selectedConvenios.findIndex(c => c.id === convenioId);
            
            if (index === -1) {
                // Adicionar convênio
                selectedConvenios.push({
                    id: convenioId,
                    nome: convenio.nome,
                    valor: 0,
                    documento: convenio.documento
                });
            } else {
                // Remover convênio
                selectedConvenios.splice(index, 1);
            }
            
            // Atualizar interface
            updateConveniosSelectionUI();
            updateResumoFechamento();
        }
        
        function updateConvenioValor(convenioId, valor) {
            const convenioIndex = selectedConvenios.findIndex(c => c.id === convenioId);
            if (convenioIndex !== -1) {
                selectedConvenios[convenioIndex].valor = valor;
                updateConveniosSelectionUI();
                updateResumoFechamento();
            }
        }
        
        function updateConveniosSelectionUI() {
            // Atualizar estado visual dos cards
            document.querySelectorAll('.convenio-card').forEach(card => {
                const convenioId = parseInt(card.getAttribute('data-id'));
                const isSelected = selectedConvenios.some(c => c.id === convenioId);
                
                if (isSelected) {
                    card.classList.add('selected');
                } else {
                    card.classList.remove('selected');
                }
            });
            
            // Atualizar lista de convênios selecionados
            const container = document.getElementById('convenios-selecionados-container');
            const lista = document.getElementById('convenios-selecionados');
            const totalSpan = document.getElementById('total-convenios-selecionados');
            
            if (selectedConvenios.length === 0) {
                container.style.display = 'none';
                return;
            }
            
            container.style.display = 'block';
            
            let html = '';
            let total = 0;
            
            selectedConvenios.forEach(convenio => {
                total += convenio.valor;
                html += `
                    <div class="convenio-badge">
                        <i class="fas fa-handshake"></i>
                        ${convenio.nome}: ${formatCurrency(convenio.valor)}
                    </div>
                `;
            });
            
            lista.innerHTML = html;
            totalSpan.textContent = formatCurrency(total);
        }
        
        // Função para carregar bancos para seleção
        function loadBancosForSelection() {
            const container = document.getElementById('bancos-selecao');
            if (!container) return;
            
            const bancosAtivos = database.bancos.filter(b => b.status === 'Ativo');
            
            if (bancosAtivos.length === 0) {
                container.innerHTML = `
                    <div class="alert alert-warning">
                        <i class="fas fa-info-circle"></i> Nenhum banco cadastrado.
                        <button class="btn btn-sm btn-primary ml-2" id="cadastrar-banco-now">
                            <i class="fas fa-plus"></i> Cadastrar Primeiro Banco
                        </button>
                    </div>
                `;
                
                document.getElementById('cadastrar-banco-now').addEventListener('click', function() {
                    showBancoModal();
                });
                return;
            }
            
            let html = '<div class="convenios-grid">';
            
            bancosAtivos.forEach(banco => {
                const isSelected = selectedTaxasBancarias.some(t => t.bancoId === banco.id);
                
                html += `
                    <div class="convenio-card ${isSelected ? 'selected' : ''}" data-banco-id="${banco.id}">
                        <div class="convenio-nome">${banco.nome}</div>
                        <div class="convenio-documento">Ag: ${banco.agencia} | C/C: ${banco.conta}</div>
                        <div class="convenio-valor-input">
                            <input type="number" step="0.01" 
                                   class="convenio-valor-input-field" 
                                   data-banco-id="${banco.id}"
                                   placeholder="Valor da Taxa (R$)"
                                   value="${isSelected ? selectedTaxasBancarias.find(t => t.bancoId === banco.id).valor || '' : ''}">
                        </div>
                    </div>
                `;
            });
            
            html += '</div>';
            container.innerHTML = html;
            
            // Configurar eventos dos cards
            document.querySelectorAll('.convenio-card[data-banco-id]').forEach(card => {
                card.addEventListener('click', function() {
                    const bancoId = parseInt(this.getAttribute('data-banco-id'));
                    toggleTaxaBancariaSelection(bancoId);
                });
            });
            
            // Configurar eventos dos inputs de valor
            document.querySelectorAll('.convenio-valor-input-field[data-banco-id]').forEach(input => {
                input.addEventListener('click', function(e) {
                    e.stopPropagation();
                });
                
                input.addEventListener('input', function() {
                    const bancoId = parseInt(this.getAttribute('data-banco-id'));
                    updateTaxaBancariaValor(bancoId, parseFloat(this.value) || 0);
                });
            });
        }

        // Função para alternar seleção de taxa bancária
        function toggleTaxaBancariaSelection(bancoId) {
            const banco = database.bancos.find(b => b.id === bancoId);
            if (!banco) return;
            
            const index = selectedTaxasBancarias.findIndex(t => t.bancoId === bancoId);
            
            if (index === -1) {
                // Adicionar taxa bancária
                selectedTaxasBancarias.push({
                    bancoId: bancoId,
                    bancoNome: banco.nome,
                    valor: 0
                });
            } else {
                // Remover taxa bancária
                selectedTaxasBancarias.splice(index, 1);
            }
            
            // Atualizar interface
            updateTaxasBancariasSelectionUI();
            updateResumoFechamento();
        }

        // Função para atualizar valor da taxa bancária
        function updateTaxaBancariaValor(bancoId, valor) {
            const taxaIndex = selectedTaxasBancarias.findIndex(t => t.bancoId === bancoId);
            if (taxaIndex !== -1) {
                selectedTaxasBancarias[taxaIndex].valor = valor;
                updateTaxasBancariasSelectionUI();
                updateResumoFechamento();
            }
        }

        // Função para atualizar interface de taxas bancárias selecionadas
        function updateTaxasBancariasSelectionUI() {
            // Atualizar estado visual dos cards
            document.querySelectorAll('.convenio-card[data-banco-id]').forEach(card => {
                const bancoId = parseInt(card.getAttribute('data-banco-id'));
                const isSelected = selectedTaxasBancarias.some(t => t.bancoId === bancoId);
                
                if (isSelected) {
                    card.classList.add('selected');
                } else {
                    card.classList.remove('selected');
                }
            });
            
            // Atualizar lista de taxas selecionadas
            const container = document.getElementById('taxas-selecionadas-container');
            const lista = document.getElementById('taxas-selecionadas');
            const totalSpan = document.getElementById('total-taxas-selecionadas');
            
            if (selectedTaxasBancarias.length === 0) {
                container.style.display = 'none';
                return;
            }
            
            container.style.display = 'block';
            
            let html = '';
            let total = 0;
            
            selectedTaxasBancarias.forEach(taxa => {
                total += taxa.valor;
                html += `
                    <div class="convenio-badge" style="background: #f8d7da; color: #721c24;">
                        <i class="fas fa-university"></i>
                        ${taxa.bancoNome}: ${formatCurrency(taxa.valor)}
                    </div>
                `;
            });
            
            lista.innerHTML = html;
            totalSpan.textContent = formatCurrency(total);
        }
        
        function updateResumoFechamento() {
            // Coletar valores básicos
            const dinheiro = parseFloat(document.getElementById('fechamento-dinheiro').value) || 0;
            const pix = parseFloat(document.getElementById('fechamento-pix').value) || 0;
            const outros = parseFloat(document.getElementById('fechamento-outros').value) || 0;
            
            // Calcular total de convênios
            const totalConvenios = selectedConvenios.reduce((sum, c) => sum + c.valor, 0);
            
            // Calcular total de taxas bancárias
            const totalTaxasBancarias = selectedTaxasBancarias.reduce((sum, t) => sum + t.valor, 0);
            
            // Para este exemplo, vamos considerar despesas gerais como 10% do faturamento
            const totalFaturamentos = dinheiro + pix + outros + totalConvenios;
            const despesasGerais = totalFaturamentos * 0.1;
            const totalLiquido = totalFaturamentos - despesasGerais - totalTaxasBancarias;
            
            // Atualizar exibição
            document.getElementById('resumo-dinheiro').textContent = formatCurrency(dinheiro);
            document.getElementById('resumo-pix').textContent = formatCurrency(pix);
            document.getElementById('resumo-outros').textContent = formatCurrency(outros);
            document.getElementById('resumo-convenios').textContent = formatCurrency(totalConvenios);
            document.getElementById('resumo-total-faturamentos').textContent = formatCurrency(totalFaturamentos);
            document.getElementById('resumo-despesas').textContent = formatCurrency(despesasGerais);
            document.getElementById('resumo-taxas-bancarias').textContent = formatCurrency(totalTaxasBancarias);
            document.getElementById('resumo-total-liquido').textContent = formatCurrency(totalLiquido);
        }
        
        function saveFechamento() {
            // Coletar valores
            const dinheiro = parseFloat(document.getElementById('fechamento-dinheiro').value) || 0;
            const pix = parseFloat(document.getElementById('fechamento-pix').value) || 0;
            const outros = parseFloat(document.getElementById('fechamento-outros').value) || 0;
            const totalConvenios = selectedConvenios.reduce((sum, c) => sum + c.valor, 0);
            const totalTaxasBancarias = selectedTaxasBancarias.reduce((sum, t) => sum + t.valor, 0);
            
            const totalFaturamentos = dinheiro + pix + outros + totalConvenios;
            const despesasGerais = totalFaturamentos * 0.1;
            const totalLiquido = totalFaturamentos - despesasGerais - totalTaxasBancarias;
            const totalDespesas = despesasGerais + totalTaxasBancarias;
            
            // Criar novo fechamento
            const novoFechamento = {
                id: database.fechamentosDiarios.length + 1,
                data: document.getElementById('fechamento-data').value,
                responsavel: document.getElementById('fechamento-responsavel').value,
                centroCusto: document.getElementById('fechamento-centro-custo').value,
                dinheiro: dinheiro,
                pix: pix,
                outrosFaturamentos: outros,
                convenios: selectedConvenios.map(c => ({
                    convenioId: c.id,
                    valor: c.valor,
                    nome: c.nome
                })),
                taxasBancarias: selectedTaxasBancarias.map(t => ({
                    bancoId: t.bancoId,
                    bancoNome: t.bancoNome,
                    valor: t.valor
                })),
                despesas: despesasGerais,
                taxasBancariasTotal: totalTaxasBancarias,
                totalFaturamentos: totalFaturamentos,
                totalDespesas: totalDespesas,
                totalDiaria: totalLiquido,
                totalLiquidoDinheiro: totalLiquido,
                status: "Fechado",
                createdAt: new Date().toISOString(),
                createdBy: currentUser.id
            };
            
            database.fechamentosDiarios.push(novoFechamento);
            
            // Atualizar interface
            document.getElementById('fechamento-form-container').style.display = 'none';
            updateFechamentosTable();
            
            // Atualizar dashboard
            if (currentPage === 'dashboard') {
                updateDashboardData();
            }
            
            showAlert('Fechamento salvo com sucesso!', 'success');
            resetFechamentoForm();
        }
        
        function updateFechamentosTable() {
            const tbody = document.getElementById('fechamentos-table');
            if (!tbody) return;
            
            // Aplicar filtro
            const filtroMes = document.getElementById('filtro-mes')?.value;
            let fechamentos = database.fechamentosDiarios;
            
            if (filtroMes) {
                const [ano, mes] = filtroMes.split('-');
                fechamentos = fechamentos.filter(f => {
                    const data = new Date(f.data);
                    return data.getFullYear() == ano && (data.getMonth() + 1) == mes;
                });
            }
            
            // Ordenar por data (mais recente primeiro)
            fechamentos.sort((a, b) => new Date(b.data) - new Date(a.data));
            
            if (fechamentos.length === 0) {
                tbody.innerHTML = '<tr><td colspan="7" class="text-center">Nenhum fechamento encontrado</td></tr>';
                return;
            }
            
            let html = '';
            fechamentos.forEach(f => {
                const totalConvenios = f.convenios ? f.convenios.reduce((sum, c) => sum + c.valor, 0) : 0;
                const conveniosInfo = f.convenios && f.convenios.length > 0 
                    ? `${f.convenios.length} convênio(s) - ${formatCurrency(totalConvenios)}`
                    : 'Nenhum';
                
                html += `
                    <tr>
                        <td>${formatDate(f.data)}</td>
                        <td>${f.responsavel}</td>
                        <td>${formatCurrency(f.dinheiro)}</td>
                        <td>${formatCurrency(f.pix)}</td>
                        <td>${conveniosInfo}</td>
                        <td><strong>${formatCurrency(f.totalDiaria)}</strong></td>
                        <td>
                            <button class="btn-icon view-fechamento" data-id="${f.id}" title="Visualizar">
                                <i class="fas fa-eye"></i>
                            </button>
                        </td>
                    </tr>
                `;
            });
            
            tbody.innerHTML = html;
        }

        // ============== SISTEMA DE CONVÊNIOS ==============
        function loadConveniosContent() {
            const page = document.getElementById('convenios');
            
            page.innerHTML = `
                <div class="d-flex justify-between mb-3">
                    <h2>Gerenciamento de Convênios</h2>
                    <button class="btn btn-primary" id="add-convenio">
                        <i class="fas fa-plus"></i> Novo Convênio
                    </button>
                </div>
                
                <!-- Filtros -->
                <div class="form-container mb-3">
                    <div class="fechamento-form">
                        <div class="fechamento-group">
                            <label class="fechamento-label">Status</label>
                            <select class="fechamento-input" id="filtro-status-convenio">
                                <option value="">Todos</option>
                                <option value="Ativo">Ativos</option>
                                <option value="Inativo">Inativos</option>
                            </select>
                        </div>
                        
                        <div class="fechamento-group">
                            <label class="fechamento-label">Centro de Custo</label>
                            <select class="fechamento-input" id="filtro-centro-convenio">
                                <option value="">Todos</option>
                                ${getCentrosCustoOptions()}
                            </select>
                        </div>
                        
                        <div class="fechamento-group">
                            <label class="fechamento-label">&nbsp;</label>
                            <button class="btn btn-primary" id="aplicar-filtro-convenios">
                                <i class="fas fa-filter"></i> Filtrar
                            </button>
                        </div>
                    </div>
                </div>
                
                <!-- Tabela de Convênios -->
                <div class="table-container">
                    <table class="table">
                        <thead>
                            <tr>
                                <th>Nome</th>
                                <th>CNPJ/CPF</th>
                                <th>Tipo</th>
                                <th>Centro de Custo</th>
                                <th>Status</th>
                                <th>Criado em</th>
                                <th>Ações</th>
                            </tr>
                        </thead>
                        <tbody id="convenios-table">
                            <!-- Dados serão carregados via JavaScript -->
                        </tbody>
                    </table>
                </div>
            `;
            
            setupConveniosEvents();
            updateConveniosTable();
        }
        
        function setupConveniosEvents() {
            // Novo convênio
            document.getElementById('add-convenio').addEventListener('click', function() {
                showConvenioModal();
            });
            
            // Aplicar filtro
            document.getElementById('aplicar-filtro-convenios').addEventListener('click', function() {
                updateConveniosTable();
            });
        }
        
        function showConvenioModal() {
            // Limpar e preparar formulário
            document.getElementById('convenio-form').reset();
            
            // Carregar opções de centros de custo
            const select = document.getElementById('convenio-centro-custo');
            select.innerHTML = '<option value="">Nenhum</option>';
            database.centrosCusto.forEach(centro => {
                const option = document.createElement('option');
                option.value = centro.codigo;
                option.textContent = `${centro.codigo} - ${centro.nome}`;
                select.appendChild(option);
            });
            
            document.getElementById('convenio-modal').style.display = 'flex';
        }
        
        function saveConvenio() {
            const nome = document.getElementById('convenio-nome').value;
            const documento = document.getElementById('convenio-documento').value;
            const tipo = document.getElementById('convenio-tipo').value;
            const centroCusto = document.getElementById('convenio-centro-custo').value;
            const status = document.getElementById('convenio-status').value;
            
            // Validar documento
            if (!validarDocumento(documento, tipo)) {
                showAlert('Documento inválido para o tipo selecionado.', 'danger');
                return;
            }
            
            const novoConvenio = {
                id: database.convenios.length + 1,
                nome: nome,
                documento: documento,
                tipo: tipo,
                centroCusto: centroCusto,
                status: status,
                createdAt: new Date().toISOString(),
                createdBy: currentUser.id
            };
            
            database.convenios.push(novoConvenio);
            
            // Fechar modal
            document.getElementById('convenio-modal').style.display = 'none';
            
            // Atualizar tabela
            updateConveniosTable();
            
            // Se estiver na tela de fechamento, recarregar convênios
            if (currentPage === 'fechamento-diario') {
                loadConveniosForSelection();
            }
            
            showAlert('Convênio cadastrado com sucesso!', 'success');
        }
        
        function validarDocumento(documento, tipo) {
            // Remover caracteres não numéricos
            const docLimpo = documento.replace(/\D/g, '');
            
            if (tipo === 'CPF') {
                return docLimpo.length === 11;
            } else if (tipo === 'CNPJ') {
                return docLimpo.length === 14;
            }
            
            return true;
        }
        
        function updateConveniosTable() {
            const tbody = document.getElementById('convenios-table');
            if (!tbody) return;
            
            // Aplicar filtros
            const filtroStatus = document.getElementById('filtro-status-convenio')?.value;
            const filtroCentro = document.getElementById('filtro-centro-convenio')?.value;
            
            let convenios = database.convenios;
            
            if (filtroStatus) {
                convenios = convenios.filter(c => c.status === filtroStatus);
            }
            
            if (filtroCentro) {
                convenios = convenios.filter(c => c.centroCusto === filtroCentro);
            }
            
            if (convenios.length === 0) {
                tbody.innerHTML = '<tr><td colspan="7" class="text-center">Nenhum convênio encontrado</td></tr>';
                return;
            }
            
            let html = '';
            convenios.forEach(c => {
                const centroNome = c.centroCusto ? 
                    database.centrosCusto.find(cc => cc.codigo === c.centroCusto)?.nome || c.centroCusto : 
                    'Não vinculado';
                
                html += `
                    <tr>
                        <td>${c.nome}</td>
                        <td>${c.documento}</td>
                        <td>${c.tipo}</td>
                        <td>${centroNome}</td>
                        <td><span class="badge ${c.status === 'Ativo' ? 'badge-success' : 'badge-danger'}">${c.status}</span></td>
                        <td>${formatDate(c.createdAt)}</td>
                        <td>
                            <button class="btn-icon edit-convenio" data-id="${c.id}" title="Editar">
                                <i class="fas fa-edit"></i>
                            </button>
                            <button class="btn-icon ${c.status === 'Ativo' ? 'deactivate-convenio' : 'activate-convenio'}" 
                                    data-id="${c.id}" title="${c.status === 'Ativo' ? 'Desativar' : 'Ativar'}">
                                <i class="fas ${c.status === 'Ativo' ? 'fa-ban' : 'fa-check'}"></i>
                            </button>
                        </td>
                    </tr>
                `;
            });
            
            tbody.innerHTML = html;
            
            // Configurar eventos dos botões
            document.querySelectorAll('.edit-convenio').forEach(btn => {
                btn.addEventListener('click', function() {
                    const id = parseInt(this.getAttribute('data-id'));
                    editConvenio(id);
                });
            });
            
            document.querySelectorAll('.deactivate-convenio, .activate-convenio').forEach(btn => {
                btn.addEventListener('click', function() {
                    const id = parseInt(this.getAttribute('data-id'));
                    toggleConvenioStatus(id);
                });
            });
        }
        
        function editConvenio(id) {
            const convenio = database.convenios.find(c => c.id === id);
            if (!convenio) return;
            
            // Preencher formulário
            document.getElementById('convenio-nome').value = convenio.nome;
            document.getElementById('convenio-documento').value = convenio.documento;
            document.getElementById('convenio-tipo').value = convenio.tipo;
            document.getElementById('convenio-status').value = convenio.status;
            
            // Carregar opções de centros de custo
            const select = document.getElementById('convenio-centro-custo');
            select.innerHTML = '<option value="">Nenhum</option>';
            database.centrosCusto.forEach(centro => {
                const option = document.createElement('option');
                option.value = centro.codigo;
                option.textContent = `${centro.codigo} - ${centro.nome}`;
                if (centro.codigo === convenio.centroCusto) {
                    option.selected = true;
                }
                select.appendChild(option);
            });
            
            // Mostrar modal
            document.getElementById('convenio-modal').style.display = 'flex';
            
            // Configurar submit para edição
            const form = document.getElementById('convenio-form');
            const originalSubmit = form.onsubmit;
            
            form.onsubmit = function(e) {
                e.preventDefault();
                
                // Atualizar convênio
                convenio.nome = document.getElementById('convenio-nome').value;
                convenio.documento = document.getElementById('convenio-documento').value;
                convenio.tipo = document.getElementById('convenio-tipo').value;
                convenio.centroCusto = document.getElementById('convenio-centro-custo').value;
                convenio.status = document.getElementById('convenio-status').value;
                
                // Fechar modal
                document.getElementById('convenio-modal').style.display = 'none';
                
                // Atualizar tabela
                updateConveniosTable();
                
                showAlert('Convênio atualizado com sucesso!', 'success');
                
                // Restaurar submit original
                form.onsubmit = originalSubmit;
            };
        }
        
        function toggleConvenioStatus(id) {
            const convenio = database.convenios.find(c => c.id === id);
            if (!convenio) return;
            
            const novaAcao = convenio.status === 'Ativo' ? 'desativar' : 'ativar';
            
            if (confirm(`Tem certeza que deseja ${novaAcao} o convênio "${convenio.nome}"?`)) {
                convenio.status = convenio.status === 'Ativo' ? 'Inativo' : 'Ativo';
                updateConveniosTable();
                showAlert(`Convênio ${novaAcao}do com sucesso!`, 'success');
            }
        }

        // ============== FUNÇÕES UTILITÁRIAS ==============
        function getCentrosCustoOptions() {
            let centros = database.centrosCusto;
            
            if (currentUser.role !== "Administrador") {
                centros = centros.filter(c => currentUser.centrosCusto.includes(c.codigo));
            }
            
            return centros.map(c => `<option value="${c.codigo}">${c.codigo} - ${c.nome}</option>`).join('');
        }
        
        function formatDate(dateString) {
            try {
                const date = new Date(dateString);
                if (isNaN(date.getTime())) return 'Data inválida';
                
                const format = database.settings.dateFormat || 'dd/mm/yyyy';
                
                if (format === 'dd/mm/yyyy') {
                    return date.toLocaleDateString('pt-BR');
                } else if (format === 'mm/dd/yyyy') {
                    return date.toLocaleDateString('en-US');
                } else {
                    return date.toISOString().split('T')[0];
                }
            } catch (e) {
                return dateString;
            }
        }
        
        function formatCurrency(value) {
            const currency = database.settings.currency || 'BRL';
            const locale = currency === 'BRL' ? 'pt-BR' : 
                          currency === 'USD' ? 'en-US' : 'de-DE';
            
            return new Intl.NumberFormat(locale, {
                style: 'currency',
                currency: currency
            }).format(value || 0);
        }
        
        function setupGlobalEvents() {
            setInterval(() => {
                const now = new Date();
                const timeString = now.toLocaleTimeString('pt-BR', { 
                    hour: '2-digit', 
                    minute: '2-digit' 
                });
                document.title = `${database.settings.systemName} | ${timeString}`;
            }, 60000);
        }

        function loadLancamentosContent() {
            const page = document.getElementById('lancamentos');
            
            page.innerHTML = `
                <div class="d-flex justify-between mb-3">
                    <h2>Gerenciamento de Lançamentos</h2>
                    <button class="btn btn-primary" id="add-lancamento-btn">
                        <i class="fas fa-plus"></i> Novo Lançamento
                    </button>
                </div>
                
                <!-- Filtros -->
                <div class="form-container mb-3">
                    <div class="fechamento-form">
                        <div class="fechamento-group">
                            <label class="fechamento-label">Data Inicial</label>
                            <input type="date" class="fechamento-input" id="filtro-data-inicio">
                        </div>
                        
                        <div class="fechamento-group">
                            <label class="fechamento-label">Data Final</label>
                            <input type="date" class="fechamento-input" id="filtro-data-fim">
                        </div>
                        
                        <div class="fechamento-group">
                            <label class="fechamento-label">Centro de Custo</label>
                            <select class="fechamento-input" id="filtro-centro-lancamento">
                                <option value="">Todos</option>
                                ${getCentrosCustoOptions()}
                            </select>
                        </div>
                        
                        <div class="fechamento-group">
                            <label class="fechamento-label">&nbsp;</label>
                            <button class="btn btn-primary" id="aplicar-filtro-lancamentos">
                                <i class="fas fa-filter"></i> Filtrar
                            </button>
                            <button class="btn btn-secondary" id="limpar-filtro-lancamentos">
                                <i class="fas fa-eraser"></i> Limpar
                            </button>
                        </div>
                    </div>
                </div>
                
                <!-- Tabela de Lançamentos -->
                <div class="table-container">
                    <table class="table">
                        <thead>
                            <tr>
                                <th>ID</th>
                                <th>Data</th>
                                <th>Descrição</th>
                                <th>Centro de Custo</th>
                                <th>Valor (R$)</th>
                                <th>Categoria</th>
                                <th>Criado por</th>
                                <th>Ações</th>
                            </tr>
                        </thead>
                        <tbody id="lancamentos-table">
                            <!-- Dados serão carregados via JavaScript -->
                        </tbody>
                    </table>
                </div>
                
                <!-- Formulário de Lançamento (inicialmente oculto) -->
                <div id="lancamento-form-container" style="display: none;">
                    <div class="form-container">
                        <h3 class="form-title" id="lancamento-form-title">Novo Lançamento</h3>
                        <form id="lancamento-form">
                            <input type="hidden" id="lancamento-id" value="">
                            
                            <div class="form-group">
                                <label class="form-label" for="lancamento-descricao">Descrição *</label>
                                <input type="text" class="form-control" id="lancamento-descricao" required>
                            </div>
                            
                            <div class="form-group">
                                <label class="form-label" for="lancamento-valor">Valor (R$) *</label>
                                <input type="number" step="0.01" class="form-control" id="lancamento-valor" required>
                            </div>
                            
                            <div class="form-group">
                                <label class="form-label" for="lancamento-centro-custo">Centro de Custo *</label>
                                <select class="form-control" id="lancamento-centro-custo" required>
                                    <option value="">Selecione...</option>
                                    ${getCentrosCustoOptions()}
                                </select>
                            </div>
                            
                            <div class="form-group">
                                <label class="form-label" for="lancamento-categoria">Categoria *</label>
                                <select class="form-control" id="lancamento-categoria" required>
                                    <option value="">Selecione...</option>
                                    <option value="Despesa">Despesa</option>
                                    <option value="Receita">Receita</option>
                                    <option value="Investimento">Investimento</option>
                                    <option value="Manutenção">Manutenção</option>
                                </select>
                            </div>
                            
                            <div class="form-group">
                                <label class="form-label" for="lancamento-data">Data *</label>
                                <input type="date" class="form-control" id="lancamento-data" required>
                            </div>
                            
                            <div class="d-flex justify-between mt-3">
                                <button type="button" id="cancel-lancamento" class="btn">Cancelar</button>
                                <button type="submit" class="btn btn-primary">Salvar Lançamento</button>
                            </div>
                        </form>
                    </div>
                </div>
            `;
            
            setupLancamentosEvents();
            updateLancamentosTable();
        }

        function setupLancamentosEvents() {
            // Novo lançamento
            document.getElementById('add-lancamento-btn').addEventListener('click', function() {
                document.getElementById('lancamento-form-container').style.display = 'block';
                document.getElementById('lancamento-form-title').textContent = 'Novo Lançamento';
                document.getElementById('lancamento-form').reset();
                document.getElementById('lancamento-id').value = '';
                document.getElementById('lancamento-data').valueAsDate = new Date();
            });
            
            // Cancelar lançamento
            document.getElementById('cancel-lancamento').addEventListener('click', function() {
                document.getElementById('lancamento-form-container').style.display = 'none';
            });
            
            // Aplicar filtro
            document.getElementById('aplicar-filtro-lancamentos').addEventListener('click', function() {
                updateLancamentosTable();
            });
            
            // Limpar filtro
            document.getElementById('limpar-filtro-lancamentos').addEventListener('click', function() {
                document.getElementById('filtro-data-inicio').value = '';
                document.getElementById('filtro-data-fim').value = '';
                document.getElementById('filtro-centro-lancamento').value = '';
                updateLancamentosTable();
            });
            
            // Salvar lançamento
            document.getElementById('lancamento-form').addEventListener('submit', function(e) {
                e.preventDefault();
                saveLancamento();
            });
        }

        function updateLancamentosTable() {
            const tbody = document.getElementById('lancamentos-table');
            if (!tbody) return;
            
            // Aplicar filtros
            const filtroInicio = document.getElementById('filtro-data-inicio')?.value;
            const filtroFim = document.getElementById('filtro-data-fim')?.value;
            const filtroCentro = document.getElementById('filtro-centro-lancamento')?.value;
            
            let lancamentos = database.lancamentos;
            
            // Filtrar por data
            if (filtroInicio) {
                lancamentos = lancamentos.filter(l => new Date(l.data) >= new Date(filtroInicio));
            }
            
            if (filtroFim) {
                lancamentos = lancamentos.filter(l => new Date(l.data) <= new Date(filtroFim));
            }
            
            // Filtrar por centro de custo
            if (filtroCentro) {
                lancamentos = lancamentos.filter(l => l.centroCusto === filtroCentro);
            }
            
            // Filtrar por permissão do usuário
            if (currentUser.role !== "Administrador") {
                lancamentos = lancamentos.filter(l => 
                    currentUser.centrosCusto.includes(l.centroCusto)
                );
            }
            
            // Ordenar por data (mais recente primeiro)
            lancamentos.sort((a, b) => new Date(b.data) - new Date(a.data));
            
            if (lancamentos.length === 0) {
                tbody.innerHTML = '<tr><td colspan="8" class="text-center">Nenhum lançamento encontrado</td></tr>';
                return;
            }
            
            let html = '';
            lancamentos.forEach(l => {
                const usuarioCriador = database.users.find(u => u.id === l.createdBy)?.name || 'Desconhecido';
                
                html += `
                    <tr>
                        <td>${l.id}</td>
                        <td>${formatDate(l.data)}</td>
                        <td>${l.descricao}</td>
                        <td>${l.centroCusto}</td>
                        <td>${formatCurrency(l.valor)}</td>
                        <td><span class="badge ${getCategoriaClass(l.categoria)}">${l.categoria}</span></td>
                        <td>${usuarioCriador}</td>
                        <td>
                            <button class="btn-icon edit-lancamento" data-id="${l.id}" title="Editar">
                                <i class="fas fa-edit"></i>
                            </button>
                            <button class="btn-icon delete-lancamento" data-id="${l.id}" title="Excluir">
                                <i class="fas fa-trash"></i>
                            </button>
                        </td>
                    </tr>
                `;
            });
            
            tbody.innerHTML = html;
            
            // Configurar eventos dos botões
            document.querySelectorAll('.edit-lancamento').forEach(btn => {
                btn.addEventListener('click', function() {
                    const id = parseInt(this.getAttribute('data-id'));
                    editLancamento(id);
                });
            });
            
            document.querySelectorAll('.delete-lancamento').forEach(btn => {
                btn.addEventListener('click', function() {
                    const id = parseInt(this.getAttribute('data-id'));
                    deleteLancamento(id);
                });
            });
        }

        function saveLancamento() {
            const id = document.getElementById('lancamento-id').value;
            const descricao = document.getElementById('lancamento-descricao').value;
            const valor = parseFloat(document.getElementById('lancamento-valor').value);
            const centroCusto = document.getElementById('lancamento-centro-custo').value;
            const categoria = document.getElementById('lancamento-categoria').value;
            const data = document.getElementById('lancamento-data').value;
            
            if (id) {
                // Editar lançamento existente
                const lancamento = database.lancamentos.find(l => l.id === parseInt(id));
                if (lancamento) {
                    lancamento.descricao = descricao;
                    lancamento.valor = valor;
                    lancamento.centroCusto = centroCusto;
                    lancamento.categoria = categoria;
                    lancamento.data = data;
                    
                    showAlert('Lançamento atualizado com sucesso!', 'success');
                }
            } else {
                // Criar novo lançamento
                const novoId = database.lancamentos.length > 0 ? 
                    Math.max(...database.lancamentos.map(l => l.id)) + 1 : 1;
                
                const novoLancamento = {
                    id: novoId,
                    data: data,
                    descricao: descricao,
                    centroCusto: centroCusto,
                    valor: valor,
                    categoria: categoria,
                    createdBy: currentUser.id
                };
                
                database.lancamentos.push(novoLancamento);
                showAlert('Lançamento criado com sucesso!', 'success');
            }
            
            // Atualizar interface
            document.getElementById('lancamento-form-container').style.display = 'none';
            updateLancamentosTable();
            
            // Atualizar dashboard
            if (currentPage === 'dashboard') {
                updateDashboardData();
            }
        }

        function editLancamento(id) {
            const lancamento = database.lancamentos.find(l => l.id === id);
            if (!lancamento) return;
            
            // Verificar permissão
            if (currentUser.role !== "Administrador" && !currentUser.centrosCusto.includes(lancamento.centroCusto)) {
                showAlert('Você não tem permissão para editar este lançamento!', 'warning');
                return;
            }
            
            // Preencher formulário
            document.getElementById('lancamento-id').value = lancamento.id;
            document.getElementById('lancamento-descricao').value = lancamento.descricao;
            document.getElementById('lancamento-valor').value = lancamento.valor;
            document.getElementById('lancamento-centro-custo').value = lancamento.centroCusto;
            document.getElementById('lancamento-categoria').value = lancamento.categoria;
            document.getElementById('lancamento-data').value = lancamento.data;
            
            // Mostrar formulário
            document.getElementById('lancamento-form-container').style.display = 'block';
            document.getElementById('lancamento-form-title').textContent = 'Editar Lançamento';
        }

        function deleteLancamento(id) {
            const lancamento = database.lancamentos.find(l => l.id === id);
            if (!lancamento) return;
            
            // Verificar permissão
            if (currentUser.role !== "Administrador" && !currentUser.centrosCusto.includes(lancamento.centroCusto)) {
                showAlert('Você não tem permissão para excluir este lançamento!', 'warning');
                return;
            }
            
            if (confirm('Tem certeza que deseja excluir este lançamento?')) {
                const index = database.lancamentos.findIndex(l => l.id === id);
                if (index !== -1) {
                    database.lancamentos.splice(index, 1);
                    updateLancamentosTable();
                    showAlert('Lançamento excluído com sucesso!', 'success');
                }
            }
        }

        function loadCentroCustoContent() {
            const page = document.getElementById('centro-custo');
            
            page.innerHTML = `
                <div id="restricted-centro-custo" class="restricted-access" style="display: none;">
                    <i class="fas fa-ban"></i>
                    <h3>Acesso Restrito</h3>
                    <p>Você não tem permissão para acessar todos os centros de custo.</p>
                    <p>Seu acesso está limitado aos centros de custo designados para o seu usuário.</p>
                </div>
                
                <div id="centro-custo-content">
                    <div class="d-flex justify-between mb-3">
                        <h2>Centros de Custo</h2>
                        <button class="btn btn-primary" id="add-centro-custo-btn" style="display: none;">
                            <i class="fas fa-plus"></i> Novo Centro de Custo
                        </button>
                    </div>
                    
                    <!-- Tabela de Centros de Custo -->
                    <div class="table-container">
                        <table class="table">
                            <thead>
                                <tr>
                                    <th>Código</th>
                                    <th>Nome</th>
                                    <th>Responsável</th>
                                    <th>Orçamento (R$)</th>
                                    <th>Gasto (R$)</th>
                                    <th>Saldo (R$)</th>
                                    <th>Status</th>
                                    <th>Ações</th>
                                </tr>
                            </thead>
                            <tbody id="centro-custo-table">
                                <!-- Dados serão carregados via JavaScript -->
                            </tbody>
                        </table>
                    </div>
                </div>
            `;
            
            checkCentroCustoAccess();
            updateCentroCustoTable();
        }

        function checkCentroCustoAccess() {
            const restrictedDiv = document.getElementById('restricted-centro-custo');
            const contentDiv = document.getElementById('centro-custo-content');
            const addButton = document.getElementById('add-centro-custo-btn');
            
            if (currentUser.role === "Administrador") {
                restrictedDiv.style.display = 'none';
                contentDiv.style.display = 'block';
                addButton.style.display = 'inline-block';
                
                // Configurar evento do botão de adicionar
                addButton.addEventListener('click', function() {
                    addCentroCusto();
                });
            } else {
                // Verificar se o usuário tem acesso a algum centro de custo
                if (currentUser.centrosCusto && currentUser.centrosCusto.length > 0) {
                    restrictedDiv.style.display = 'none';
                    contentDiv.style.display = 'block';
                    addButton.style.display = 'none';
                } else {
                    restrictedDiv.style.display = 'block';
                    contentDiv.style.display = 'none';
                }
            }
        }

        function updateCentroCustoTable() {
            const tbody = document.getElementById('centro-custo-table');
            if (!tbody) return;
            
            // Filtrar centros de custo por permissão do usuário
            let centrosExibidos = database.centrosCusto;
            
            if (currentUser.role !== "Administrador") {
                centrosExibidos = database.centrosCusto.filter(c => 
                    currentUser.centrosCusto.includes(c.codigo)
                );
            }
            
            if (centrosExibidos.length === 0) {
                tbody.innerHTML = '<tr><td colspan="8" class="text-center">Nenhum centro de custo encontrado</td></tr>';
                return;
            }
            
            let html = '';
            centrosExibidos.forEach(centro => {
                const saldo = centro.orcamento - centro.gasto;
                const percentualUso = centro.orcamento > 0 ? (centro.gasto / centro.orcamento) * 100 : 0;
                
                html += `
                    <tr>
                        <td><strong>${centro.codigo}</strong></td>
                        <td>${centro.nome}</td>
                        <td>${centro.responsavel}</td>
                        <td>${formatCurrency(centro.orcamento)}</td>
                        <td>${formatCurrency(centro.gasto)}</td>
                        <td class="${saldo < 0 ? 'danger-text' : 'success-text'}">${formatCurrency(saldo)}</td>
                        <td>
                            <span class="badge ${centro.status === 'Ativo' ? 'badge-success' : 'badge-danger'}">
                                ${centro.status}
                            </span>
                            <div class="progress" style="height: 5px; width: 100px; margin-top: 5px;">
                                <div class="progress-bar ${percentualUso > 90 ? 'bg-danger' : percentualUso > 70 ? 'bg-warning' : 'bg-success'}" 
                                     style="width: ${Math.min(percentualUso, 100)}%"></div>
                            </div>
                        </td>
                        <td>
                            <button class="btn-icon edit-centro" data-codigo="${centro.codigo}" title="Editar">
                                <i class="fas fa-edit"></i>
                            </button>
                            ${currentUser.role === "Administrador" ? `
                            <button class="btn-icon toggle-centro" data-codigo="${centro.codigo}" 
                                    title="${centro.status === 'Ativo' ? 'Desativar' : 'Ativar'}">
                                <i class="fas ${centro.status === 'Ativo' ? 'fa-ban' : 'fa-check'}"></i>
                            </button>` : ''}
                        </td>
                    </tr>
                `;
            });
            
            tbody.innerHTML = html;
            
            // Configurar eventos dos botões
            document.querySelectorAll('.edit-centro').forEach(btn => {
                btn.addEventListener('click', function() {
                    const codigo = this.getAttribute('data-codigo');
                    editCentroCusto(codigo);
                });
            });
            
            document.querySelectorAll('.toggle-centro').forEach(btn => {
                btn.addEventListener('click', function() {
                    const codigo = this.getAttribute('data-codigo');
                    toggleCentroCustoStatus(codigo);
                });
            });
        }

        function addCentroCusto() {
            // Implementar adição de centro de custo
            showAlert('Funcionalidade de adicionar centro de custo em desenvolvimento', 'info');
        }

        function editCentroCusto(codigo) {
            // Implementar edição de centro de custo
            showAlert('Funcionalidade de editar centro de custo em desenvolvimento', 'info');
        }

        function toggleCentroCustoStatus(codigo) {
            const centro = database.centrosCusto.find(c => c.codigo === codigo);
            if (!centro) return;
            
            const novaAcao = centro.status === 'Ativo' ? 'desativar' : 'ativar';
            
            if (confirm(`Tem certeza que deseja ${novaAcao} o centro de custo "${centro.nome}"?`)) {
                centro.status = centro.status === 'Ativo' ? 'Inativo' : 'Ativo';
                updateCentroCustoTable();
                showAlert(`Centro de custo ${novaAcao}do com sucesso!`, 'success');
            }
        }
        
        function loadUploadExcelContent() {
            const page = document.getElementById('upload-excel');
            
            page.innerHTML = `
                <div class="form-container">
                    <h3 class="form-title">Importar Dados do Excel</h3>
                    
                    <div class="upload-area" id="drop-area">
                        <div class="upload-icon">
                            <i class="fas fa-file-excel"></i>
                        </div>
                        <h3>Arraste e solte seu arquivo Excel aqui</h3>
                        <p>ou</p>
                        <button class="btn btn-primary" id="browse-btn">Selecione o arquivo</button>
                        <input type="file" id="file-input" accept=".xlsx, .xls" style="display: none;">
                    </div>
                    
                    <div class="alert alert-warning">
                        <i class="fas fa-info-circle"></i> O arquivo Excel deve conter as colunas: Data, Descrição, Valor, Centro de Custo, Categoria
                    </div>
                    
                    <div id="file-info" style="display: none;">
                        <div class="form-group">
                            <label class="form-label">Arquivo selecionado:</label>
                            <div id="selected-file-name" class="text-success"></div>
                        </div>
                        
                        <div class="form-group">
                            <label class="form-label" for="sheet-select">Planilha:</label>
                            <select class="form-control" id="sheet-select">
                                <option>Planilha1</option>
                            </select>
                        </div>
                        
                        <div class="form-group">
                            <label class="form-label" for="start-row">Linha de início dos dados:</label>
                            <input type="number" class="form-control" id="start-row" value="2" min="1">
                        </div>
                        
                        <button class="btn btn-success btn-block" id="import-btn">
                            <i class="fas fa-upload"></i> Importar Dados
                        </button>
                    </div>
                    
                    <div id="import-results" style="display: none; margin-top: 20px;"></div>
                </div>
            `;
            
            setupFileUpload();
        }

        function setupFileUpload() {
            const dropArea = document.getElementById('drop-area');
            const fileInput = document.getElementById('file-input');
            const browseBtn = document.getElementById('browse-btn');
            
            if (!dropArea || !fileInput || !browseBtn) return;
            
            // Prevenir comportamentos padrão para drag and drop
            ['dragenter', 'dragover', 'dragleave', 'drop'].forEach(eventName => {
                dropArea.addEventListener(eventName, preventDefaults, false);
            });
            
            function preventDefaults(e) {
                e.preventDefault();
                e.stopPropagation();
            }
            
            // Destacar área de drop
            ['dragenter', 'dragover'].forEach(eventName => {
                dropArea.addEventListener(eventName, highlight, false);
            });
            
            ['dragleave', 'drop'].forEach(eventName => {
                dropArea.addEventListener(eventName, unhighlight, false);
            });
            
            function highlight() {
                dropArea.style.borderColor = '#3498db';
                dropArea.style.backgroundColor = 'rgba(52, 152, 219, 0.05)';
            }
            
            function unhighlight() {
                dropArea.style.borderColor = '#ddd';
                dropArea.style.backgroundColor = '';
            }
            
            // Lidar com arquivo dropado
            dropArea.addEventListener('drop', handleDrop, false);
            
            function handleDrop(e) {
                const dt = e.dataTransfer;
                const files = dt.files;
                
                if (files.length) {
                    handleFiles(files);
                }
            }
            
            // Botão para selecionar arquivo
            browseBtn.addEventListener('click', function() {
                fileInput.click();
            });
            
            // Lidar com seleção de arquivo
            fileInput.addEventListener('change', function() {
                if (this.files.length) {
                    handleFiles(this.files);
                }
            });
            
            // Botão de importação
            const importBtn = document.getElementById('import-btn');
            if (importBtn) {
                importBtn.addEventListener('click', simulateExcelImport);
            }
        }

        function handleFiles(files) {
            const file = files[0];
            
            // Verificar se é um arquivo Excel
            if (!file.name.match(/\.(xlsx|xls)$/i)) {
                alert('Por favor, selecione um arquivo Excel (.xlsx ou .xls)');
                return;
            }
            
            // Mostrar informações do arquivo
            document.getElementById('selected-file-name').textContent = 
                file.name + ' (' + (file.size / 1024).toFixed(2) + ' KB)';
            document.getElementById('file-info').style.display = 'block';
        }

        function simulateExcelImport() {
            // Simular processamento
            const importResults = document.getElementById('import-results');
            importResults.innerHTML = `
                <div class="alert alert-success">
                    <i class="fas fa-check-circle"></i> Importação concluída com sucesso!<br><br>
                    <strong>15 registros importados</strong><br>
                    <strong>2 registros com erro</strong> (verifique os dados)<br><br>
                    <button class="btn btn-sm btn-primary" id="ver-detalhes">Ver detalhes da importação</button>
                </div>
            `;
            importResults.style.display = 'block';
            
            // Simular adição de novos dados ao banco
            for (let i = 0; i < 5; i++) {
                const newId = database.lancamentos.length > 0 ? 
                    Math.max(...database.lancamentos.map(l => l.id)) + 1 : 1;
                
                const novoLancamento = {
                    id: newId,
                    data: `2026-01-${15 + i}`,
                    descricao: `Lançamento importado ${i + 1}`,
                    centroCusto: ['ADM', 'VENDAS', 'PROD', 'TI', 'RH'][i % 5],
                    valor: Math.random() * 5000 + 1000,
                    categoria: ['Despesa', 'Receita', 'Investimento'][i % 3],
                    createdBy: currentUser.id
                };
                
                database.lancamentos.push(novoLancamento);
            }
            
            showAlert('Dados do Excel importados com sucesso!', 'success');
            
            // Atualizar páginas se necessário
            if (currentPage === 'lancamentos') {
                updateLancamentosTable();
            }
            
            // Configurar botão de detalhes
            document.getElementById('ver-detalhes').addEventListener('click', function() {
                alert('Detalhes da importação:\n\n' +
                      '• 15 registros processados\n' +
                      '• 13 registros importados com sucesso\n' +
                      '• 2 registros com erro de formatação\n' +
                      '• Dados atualizados no sistema');
            });
        }
        
        function loadRelatoriosContent() {
            const page = document.getElementById('relatorios');
            
            page.innerHTML = `
                <h2>Relatórios Personalizáveis</h2>
                
                <div class="dashboard-customizer mb-3">
                    <h3>Construtor de Relatórios</h3>
                    <p>Selecione os dados e visualize seu relatório personalizado</p>
                    
                    <div class="fechamento-form">
                        <div class="fechamento-group">
                            <label class="fechamento-label">Tipo de Gráfico</label>
                            <select class="fechamento-input" id="chart-type">
                                <option value="bar">Gráfico de Barras</option>
                                <option value="line">Gráfico de Linhas</option>
                                <option value="pie">Gráfico de Pizza</option>
                                <option value="doughnut">Gráfico de Rosca</option>
                            </select>
                        </div>
                        
                        <div class="fechamento-group">
                            <label class="fechamento-label">Eixo X (Categorias)</label>
                            <select class="fechamento-input" id="x-axis">
                                <option value="categoria">Categoria</option>
                                <option value="centro_custo">Centro de Custo</option>
                                <option value="mes">Mês</option>
                            </select>
                        </div>
                        
                        <div class="fechamento-group">
                            <label class="fechamento-label">Eixo Y (Valores)</label>
                            <select class="fechamento-input" id="y-axis">
                                <option value="valor">Valor Total</option>
                                <option value="quantidade">Quantidade</option>
                            </select>
                        </div>
                    </div>
                    
                    <div class="fechamento-form">
                        <div class="fechamento-group">
                            <label class="fechamento-label">Período</label>
                            <input type="month" class="fechamento-input" id="report-month" 
                                   value="${new Date().toISOString().substring(0, 7)}">
                        </div>
                        
                        <div class="fechamento-group">
                            <label class="fechamento-label">&nbsp;</label>
                            <button class="btn btn-primary" id="generate-report">
                                <i class="fas fa-chart-bar"></i> Gerar Relatório
                            </button>
                        </div>
                    </div>
                </div>
                
                <div class="chart-container">
                    <div class="chart-header">
                        <div class="chart-title">Relatório Personalizado</div>
                        <button class="btn btn-sm btn-secondary" id="export-report">
                            <i class="fas fa-download"></i> Exportar
                        </button>
                    </div>
                    <canvas id="customReportChart"></canvas>
                </div>
                
                <div class="table-container mt-3" id="report-table-container" style="display: none;">
                    <h4>Dados do Relatório</h4>
                    <table class="table">
                        <thead id="report-table-head">
                            <!-- Cabeçalho será gerado dinamicamente -->
                        </thead>
                        <tbody id="report-table-body">
                            <!-- Dados serão gerados dinamicamente -->
                        </tbody>
                    </table>
                </div>
            `;
            setupReportGenerator();
        }

        function setupReportGenerator() {
            document.getElementById('generate-report').addEventListener('click', function() {
                generateReport();
            });
            
            document.getElementById('export-report').addEventListener('click', function() {
                exportReport();
            });
        }

        function generateReport() {
            const chartType = document.getElementById('chart-type').value;
            const xAxis = document.getElementById('x-axis').value;
            const yAxis = document.getElementById('y-axis').value;
            const month = document.getElementById('report-month').value;
            
            // Gerar dados de exemplo baseado nas seleções
            let labels = [];
            let data = [];
            let tableData = [];
            
            if (xAxis === 'categoria') {
                labels = ['Despesa', 'Receita', 'Investimento', 'Manutenção'];
                data = [65000, 32000, 15000, 8000];
                tableData = [
                    { categoria: 'Despesa', valor: 65000, quantidade: 45 },
                    { categoria: 'Receita', valor: 32000, quantidade: 12 },
                    { categoria: 'Investimento', valor: 15000, quantidade: 5 },
                    { categoria: 'Manutenção', valor: 8000, quantidade: 8 }
                ];
            } else if (xAxis === 'centro_custo') {
                labels = database.centrosCusto.map(c => c.nome);
                data = database.centrosCusto.map(c => c.gasto);
                tableData = database.centrosCusto.map(c => ({
                    centro: c.nome,
                    orcamento: c.orcamento,
                    gasto: c.gasto,
                    saldo: c.orcamento - c.gasto
                }));
            } else if (xAxis === 'mes') {
                labels = ['Jan', 'Fev', 'Mar', 'Abr', 'Mai', 'Jun'];
                data = [12000, 19000, 15000, 25000, 22000, 30000];
                tableData = labels.map((mes, i) => ({
                    mes: mes,
                    valor: data[i],
                    variacao: i > 0 ? ((data[i] - data[i-1]) / data[i-1] * 100).toFixed(1) + '%' : '-'
                }));
            }
            
            // Atualizar o gráfico
            const chartCanvas = document.getElementById('customReportChart');
            const ctx = chartCanvas.getContext('2d');
            
            // Destruir gráfico anterior se existir
            if (window.reportChart) {
                window.reportChart.destroy();
            }
            
            window.reportChart = new Chart(ctx, {
                type: chartType,
                data: {
                    labels: labels,
                    datasets: [{
                        label: yAxis === 'valor' ? 'Valor Total (R$)' : 'Quantidade',
                        data: data,
                        backgroundColor: chartType === 'line' ? 
                            'rgba(52, 152, 219, 0.1)' : 
                            ['rgba(52, 152, 219, 0.7)', 'rgba(39, 174, 96, 0.7)', 'rgba(241, 196, 15, 0.7)', 'rgba(155, 89, 182, 0.7)', 'rgba(231, 76, 60, 0.7)'],
                        borderColor: 'rgba(52, 152, 219, 1)',
                        borderWidth: chartType === 'line' ? 3 : 1,
                        fill: chartType === 'line'
                    }]
                },
                options: {
                    responsive: true,
                    scales: chartType !== 'pie' && chartType !== 'doughnut' ? {
                        y: {
                            beginAtZero: true,
                            ticks: {
                                callback: function(value) {
                                    return yAxis === 'quantidade' ? 
                                        value : 'R$ ' + value.toLocaleString('pt-BR');
                                }
                            }
                        }
                    } : {}
                }
            });
            
            // Atualizar tabela de dados
            updateReportTable(xAxis, tableData);
            
            showAlert('Relatório gerado com sucesso!', 'success');
        }

        function updateReportTable(xAxis, data) {
            const container = document.getElementById('report-table-container');
            const thead = document.getElementById('report-table-head');
            const tbody = document.getElementById('report-table-body');
            
            container.style.display = 'block';
            thead.innerHTML = '';
            tbody.innerHTML = '';
            
            let headers = [];
            if (xAxis === 'categoria') {
                headers = ['Categoria', 'Valor Total (R$)', 'Quantidade'];
            } else if (xAxis === 'centro_custo') {
                headers = ['Centro de Custo', 'Orçamento (R$)', 'Gasto (R$)', 'Saldo (R$)'];
            } else if (xAxis === 'mes') {
                headers = ['Mês', 'Valor (R$)', 'Variação'];
            }
            
            // Criar cabeçalho
            let headerRow = '<tr>';
            headers.forEach(header => {
                headerRow += `<th>${header}</th>`;
            });
            headerRow += '</tr>';
            thead.innerHTML = headerRow;
            
            // Criar linhas de dados
            data.forEach(item => {
                let row = '<tr>';
                if (xAxis === 'categoria') {
                    row += `<td>${item.categoria}</td>`;
                    row += `<td>${formatCurrency(item.valor)}</td>`;
                    row += `<td>${item.quantidade}</td>`;
                } else if (xAxis === 'centro_custo') {
                    row += `<td>${item.centro}</td>`;
                    row += `<td>${formatCurrency(item.orcamento)}</td>`;
                    row += `<td>${formatCurrency(item.gasto)}</td>`;
                    row += `<td class="${item.saldo < 0 ? 'danger-text' : 'success-text'}">${formatCurrency(item.saldo)}</td>`;
                } else if (xAxis === 'mes') {
                    row += `<td>${item.mes}</td>`;
                    row += `<td>${formatCurrency(item.valor)}</td>`;
                    row += `<td>${item.variacao}</td>`;
                }
                row += '</tr>';
                tbody.innerHTML += row;
            });
        }

        function exportReport() {
            // Simular exportação
            showAlert('Relatório exportado com sucesso! (simulação)', 'success');
        }
        
        function loadUsuariosContent() {
            const page = document.getElementById('usuarios');
            
            page.innerHTML = `
                <div id="restricted-usuarios" class="restricted-access" style="display: none;">
                    <i class="fas fa-ban"></i>
                    <h3>Acesso Restrito</h3>
                    <p>Você não tem permissão para gerenciar usuários.</p>
                    <p>Apenas administradores podem acessar esta funcionalidade.</p>
                </div>
                
                <div id="usuarios-content">
                    <div class="d-flex justify-between mb-3">
                        <h2>Gerenciamento de Usuários</h2>
                        <button class="btn btn-primary" id="add-usuario-btn">
                            <i class="fas fa-user-plus"></i> Novo Usuário
                        </button>
                    </div>
                    
                    <!-- Tabela de Usuários -->
                    <div class="table-container">
                        <table class="table">
                            <thead>
                                <tr>
                                    <th>ID</th>
                                    <th>Nome</th>
                                    <th>Email</th>
                                    <th>Perfil</th>
                                    <th>Centros de Custo</th>
                                    <th>Último Acesso</th>
                                    <th>Status</th>
                                    <th>Ações</th>
                                </tr>
                            </thead>
                            <tbody id="usuarios-table">
                                <!-- Dados serão carregados via JavaScript -->
                            </tbody>
                        </table>
                    </div>
                </div>
            `;
            
            checkUsuariosAccess();
            updateUsuariosTable();
        }

        function checkUsuariosAccess() {
            const restrictedDiv = document.getElementById('restricted-usuarios');
            const contentDiv = document.getElementById('usuarios-content');
            
            if (currentUser.role === "Administrador") {
                restrictedDiv.style.display = 'none';
                contentDiv.style.display = 'block';
                
                // Configurar evento do botão de adicionar
                document.getElementById('add-usuario-btn').addEventListener('click', function() {
                    addUsuario();
                });
            } else {
                restrictedDiv.style.display = 'block';
                contentDiv.style.display = 'none';
            }
        }

        function updateUsuariosTable() {
            const tbody = document.getElementById('usuarios-table');
            if (!tbody) return;
            
            // Filtrar usuários (administrador vê todos, outros só veem a si mesmos)
            let usuariosExibidos = database.users;
            
            if (currentUser.role !== "Administrador") {
                usuariosExibidos = database.users.filter(u => u.id === currentUser.id);
            }
            
            if (usuariosExibidos.length === 0) {
                tbody.innerHTML = '<tr><td colspan="8" class="text-center">Nenhum usuário encontrado</td></tr>';
                return;
            }
            
            let html = '';
            usuariosExibidos.forEach(user => {
                // Mostrar centros de custo do usuário
                const centrosText = user.centrosCusto && user.centrosCusto.length > 0 
                    ? user.centrosCusto.join(', ')
                    : 'Nenhum';
                
                html += `
                    <tr>
                        <td>${user.id}</td>
                        <td>${user.name}</td>
                        <td>${user.email}</td>
                        <td><span class="badge ${user.role === 'Administrador' ? 'badge-danger' : 'badge-primary'}">${user.role}</span></td>
                        <td title="${centrosText}">${centrosText.length > 30 ? centrosText.substring(0, 30) + '...' : centrosText}</td>
                        <td>${formatDate(user.lastLogin)}</td>
                        <td><span class="badge ${user.status === 'Ativo' ? 'badge-success' : 'badge-danger'}">${user.status}</span></td>
                        <td>
                            <button class="btn-icon edit-usuario" data-id="${user.id}" title="Editar">
                                <i class="fas fa-edit"></i>
                            </button>
                            ${currentUser.role === "Administrador" && user.id !== currentUser.id ? `
                            <button class="btn-icon toggle-usuario" data-id="${user.id}" 
                                    title="${user.status === 'Ativo' ? 'Desativar' : 'Ativar'}">
                                <i class="fas ${user.status === 'Ativo' ? 'fa-user-slash' : 'fa-user-check'}"></i>
                            </button>
                            ` : ''}
                        </td>
                    </tr>
                `;
            });
            
            tbody.innerHTML = html;
            
            // Configurar eventos dos botões
            document.querySelectorAll('.edit-usuario').forEach(btn => {
                btn.addEventListener('click', function() {
                    const id = parseInt(this.getAttribute('data-id'));
                    editUsuario(id);
                });
            });
            
            document.querySelectorAll('.toggle-usuario').forEach(btn => {
                btn.addEventListener('click', function() {
                    const id = parseInt(this.getAttribute('data-id'));
                    toggleUsuarioStatus(id);
                });
            });
        }

        function addUsuario() {
            // Implementar adição de usuário
            showAlert('Funcionalidade de adicionar usuário em desenvolvimento', 'info');
        }

        function editUsuario(id) {
            // Implementar edição de usuário
            showAlert('Funcionalidade de editar usuário em desenvolvimento', 'info');
        }

        function toggleUsuarioStatus(id) {
            const usuario = database.users.find(u => u.id === id);
            if (!usuario) return;
            
            const novaAcao = usuario.status === 'Ativo' ? 'desativar' : 'ativar';
            
            if (confirm(`Tem certeza que deseja ${novaAcao} o usuário "${usuario.name}"?`)) {
                usuario.status = usuario.status === 'Ativo' ? 'Inativo' : 'Ativo';
                updateUsuariosTable();
                showAlert(`Usuário ${novaAcao}do com sucesso!`, 'success');
            }
        }

        // ============== PÁGINA: CONFIGURAÇÕES ==============
        function loadConfiguracoesContent() {
            const page = document.getElementById('configuracoes');
            
            page.innerHTML = `
                <div id="restricted-config" class="restricted-access" style="display: none;">
                    <i class="fas fa-ban"></i>
                    <h3>Acesso Restrito</h3>
                    <p>Você não tem permissão para alterar as configurações do sistema.</p>
                    <p>Apenas administradores podem acessar esta funcionalidade.</p>
                </div>
                
                <div id="config-content">
                    <div class="form-container">
                        <h3 class="form-title">Configurações do Sistema</h3>
                        
                        <div class="form-group">
                            <label class="form-label" for="system-name">Nome do Sistema</label>
                            <input type="text" class="form-control" id="system-name" value="${database.settings.systemName}">
                        </div>
                        
                        <div class="form-group">
                            <label class="form-label" for="system-currency">Moeda Padrão</label>
                            <select class="form-control" id="system-currency">
                                <option value="BRL" ${database.settings.currency === 'BRL' ? 'selected' : ''}>Real Brasileiro (R$)</option>
                                <option value="USD" ${database.settings.currency === 'USD' ? 'selected' : ''}>Dólar Americano ($)</option>
                                <option value="EUR" ${database.settings.currency === 'EUR' ? 'selected' : ''}>Euro (€)</option>
                            </select>
                        </div>
                        
                        <div class="form-group">
                            <label class="form-label" for="date-format">Formato de Data</label>
                            <select class="form-control" id="date-format">
                                <option value="dd/mm/yyyy" ${database.settings.dateFormat === 'dd/mm/yyyy' ? 'selected' : ''}>DD/MM/YYYY</option>
                                <option value="mm/dd/yyyy" ${database.settings.dateFormat === 'mm/dd/yyyy' ? 'selected' : ''}>MM/DD/YYYY</option>
                                <option value="yyyy-mm-dd" ${database.settings.dateFormat === 'yyyy-mm-dd' ? 'selected' : ''}>YYYY-MM-DD</option>
                            </select>
                        </div>
                        
                        <div class="form-group">
                            <label class="form-label" for="session-timeout">Tempo de Sessão (minutos)</label>
                            <input type="number" class="form-control" id="session-timeout" 
                                   value="${database.settings.sessionTimeout}" min="5" max="120">
                        </div>
                        
                        <div class="form-group">
                            <div class="form-check">
                                <input type="checkbox" class="form-check-input" id="email-notifications" 
                                       ${database.settings.emailNotifications ? 'checked' : ''}>
                                <label class="form-check-label" for="email-notifications">Ativar notificações por email</label>
                            </div>
                        </div>
                        
                        <div class="form-group">
                            <div class="form-check">
                                <input type="checkbox" class="form-check-input" id="backup-automatico" 
                                       ${database.settings.backupAutomatico ? 'checked' : ''}>
                                <label class="form-check-label" for="backup-automatico">Backup automático diário</label>
                            </div>
                        </div>
                        
                        <div class="alert alert-info">
                            <i class="fas fa-info-circle"></i> As alterações serão aplicadas após salvar e recarregar o sistema.
                        </div>
                        
                        <button class="btn btn-primary btn-block" id="save-settings">
                            <i class="fas fa-save"></i> Salvar Configurações
                        </button>
                    </div>
                </div>
            `;
            
            checkConfigAccess();
            setupConfigEvents();
        }

        function checkConfigAccess() {
            const restrictedDiv = document.getElementById('restricted-config');
            const contentDiv = document.getElementById('config-content');
            
            if (currentUser.role === "Administrador") {
                restrictedDiv.style.display = 'none';
                contentDiv.style.display = 'block';
            } else {
                restrictedDiv.style.display = 'block';
                contentDiv.style.display = 'none';
            }
        }

        function setupConfigEvents() {
            document.getElementById('save-settings').addEventListener('click', function() {
                saveSettings();
            });
        }

        function saveSettings() {
            // Coletar valores
            database.settings.systemName = document.getElementById('system-name').value;
            database.settings.currency = document.getElementById('system-currency').value;
            database.settings.dateFormat = document.getElementById('date-format').value;
            database.settings.sessionTimeout = parseInt(document.getElementById('session-timeout').value);
            database.settings.emailNotifications = document.getElementById('email-notifications').checked;
            database.settings.backupAutomatico = document.getElementById('backup-automatico').checked;
            
            // Atualizar timer da sessão se necessário
            if (sessionTimer) {
                clearTimeout(sessionTimer);
                startSessionTimer();
            }
            
            showAlert('Configurações salvas com sucesso!', 'success');
            
            // Atualizar título da página se necessário
            document.title = `${database.settings.systemName} | Sistema de Gerenciamento`;
        }

        // ============== FUNÇÕES AUXILIARES ==============
        function getCategoriaClass(categoria) {
            const classes = {
                'Despesa': 'badge-danger',
                'Receita': 'badge-success',
                'Investimento': 'badge-primary',
                'Manutenção': 'badge-warning'
            };
            
            return classes[categoria] || 'badge-secondary';
        }

        // Adicione esta função ao final do seu script, antes do fechamento </script>
        function setupRestrictedAccessElements() {
            // Adicionar estilos CSS para acesso restrito se não existirem
            if (!document.querySelector('#restricted-access-styles')) {
                const style = document.createElement('style');
                style.id = 'restricted-access-styles';
                style.textContent = `
                    .restricted-access {
                        background-color: #ffebee;
                        color: #c62828;
                        padding: 30px;
                        border-radius: 10px;
                        text-align: center;
                        margin: 20px 0;
                        border: 1px solid #ffcdd2;
                    }
                    
                    .restricted-access i {
                        font-size: 3rem;
                        margin-bottom: 15px;
                        color: #f44336;
                    }
                    
                    .restricted-access h3 {
                        margin-bottom: 10px;
                        color: #c62828;
                    }
                    
                    .restricted-access p {
                        margin-bottom: 5px;
                        color: #b71c1c;
                    }
                    
                    .badge {
                        display: inline-block;
                        padding: 5px 10px;
                        border-radius: 20px;
                        font-size: 0.8rem;
                        font-weight: 600;
                    }
                    
                    .badge-success {
                        background-color: #d4edda;
                        color: #155724;
                    }
                    
                    .badge-danger {
                        background-color: #f8d7da;
                        color: #721c24;
                    }
                    
                    .badge-warning {
                        background-color: #fff3cd;
                        color: #856404;
                    }
                    
                    .badge-primary {
                        background-color: #d1ecf1;
                        color: #0c5460;
                    }
                    
                    .badge-secondary {
                        background-color: #e2e3e5;
                        color: #383d41;
                    }
                    
                    .progress {
                        background-color: #e9ecef;
                        border-radius: 0.25rem;
                        overflow: hidden;
                    }
                    
                    .progress-bar {
                        height: 100%;
                        transition: width 0.6s ease;
                    }
                    
                    .bg-success { background-color: #28a745 !important; }
                    .bg-warning { background-color: #ffc107 !important; }
                    .bg-danger { background-color: #dc3545 !important; }
                    
                    .upload-area {
                        border: 2px dashed #ddd;
                        border-radius: 10px;
                        padding: 40px;
                        text-align: center;
                        margin-bottom: 20px;
                        transition: all 0.3s;
                        cursor: pointer;
                    }
                    
                    .upload-icon {
                        font-size: 3rem;
                        color: #bbb;
                        margin-bottom: 15px;
                    }
                `;
                document.head.appendChild(style);
            }
        }

        // ============== INICIALIZAÇÃO FINAL ==============
        console.log('Sistema DataManager 2.1 carregado com sucesso!');
        console.log('Correções aplicadas:');
        console.log('1. Dashboard agora aparece corretamente');
        console.log('2. Sistema de Convênios implementado');
        console.log('3. Abas no fechamento diário para seleção de convênios');
        console.log('4. Cadastro completo de convênios com CNPJ/CPF');
    </script>
</body>
</html>
