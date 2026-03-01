Ferramenta Auxiliar de Gestão de Balsas
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AmazonDash Pro - Sistema de Gerenciamento</title>
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
            --info: #17a2b8;
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
            gap: 8px;
        }

        .btn i {
            font-size: 1rem;
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

        .btn-warning {
            background-color: var(--warning);
            color: white;
        }

        .btn-warning:hover {
            background-color: #e67e22;
            transform: translateY(-2px);
        }

        .btn-danger {
            background-color: var(--danger);
            color: white;
        }

        .btn-danger:hover {
            background-color: #c0392b;
            transform: translateY(-2px);
        }

        .btn-sm {
            padding: 8px 16px;
            font-size: 0.9rem;
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
            transition: all 0.3s;
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

        .logo-img {
            max-height: 40px;
            max-width: 150px;
            margin-right: 10px;
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
            cursor: pointer;
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

        .nav-link.disabled {
            opacity: 0.5;
            pointer-events: none;
            cursor: not-allowed;
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

        .header-logo {
            max-height: 50px;
            max-width: 200px;
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

        .user-details {
            text-align: right;
        }

        .user-name {
            font-weight: 600;
            color: var(--primary);
        }

        .user-role {
            font-size: 0.9rem;
            color: #666;
        }

        .user-centro {
            font-size: 0.8rem;
            color: var(--secondary);
            font-weight: 600;
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

        /* FILTROS DO DASHBOARD */
        .filtros-dashboard {
            background: white;
            border-radius: 10px;
            padding: 20px;
            margin-bottom: 30px;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.05);
        }

        .filtros-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 15px;
            margin-bottom: 15px;
        }

        .filtro-group {
            display: flex;
            flex-direction: column;
        }

        .filtro-label {
            font-weight: 600;
            color: #555;
            margin-bottom: 5px;
            font-size: 0.9rem;
        }

        .filtro-label i {
            margin-right: 5px;
            color: var(--secondary);
        }

        .filtro-input {
            padding: 10px 12px;
            border: 2px solid #e0e0e0;
            border-radius: 5px;
            font-size: 0.95rem;
        }

        .filtro-input:focus {
            outline: none;
            border-color: var(--secondary);
        }

        .filtros-actions {
            display: flex;
            gap: 10px;
            justify-content: flex-end;
            margin-top: 15px;
            padding-top: 15px;
            border-top: 1px solid #e0e0e0;
        }

        .filtro-tag {
            display: inline-flex;
            align-items: center;
            background: var(--secondary);
            color: white;
            padding: 5px 12px;
            border-radius: 20px;
            font-size: 0.9rem;
            margin: 0 5px 5px 0;
        }

        .filtro-tag i {
            margin-left: 8px;
            cursor: pointer;
            font-size: 0.8rem;
        }

        .filtro-tag i:hover {
            opacity: 0.8;
        }

        .filtros-ativos {
            display: flex;
            flex-wrap: wrap;
            margin-top: 10px;
            min-height: 30px;
        }

        /* DASHBOARD */
        .dashboard-welcome {
            text-align: center;
            margin-bottom: 30px;
            padding: 30px;
            background: linear-gradient(135deg, var(--primary) 0%, var(--secondary) 100%);
            border-radius: 10px;
            color: white;
            position: relative;
            overflow: hidden;
        }

        .dashboard-welcome-logo {
            max-height: 80px;
            max-width: 200px;
            margin-bottom: 20px;
            background: white;
            padding: 10px;
            border-radius: 10px;
            display: inline-block;
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

        .info-text {
            color: var(--info);
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

        .badge-warning {
            background-color: #fff3cd;
            color: #856404;
        }

        .badge-info {
            background-color: #d1ecf1;
            color: #0c5460;
        }

        .badge-primary {
            background-color: #cce5ff;
            color: #004085;
        }

        /* ALERTAS */
        .alert {
            padding: 15px;
            border-radius: 8px;
            margin-bottom: 20px;
            display: flex;
            align-items: center;
            position: relative;
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

        .close-alert {
            position: absolute;
            right: 15px;
            top: 15px;
            background: none;
            border: none;
            cursor: pointer;
            color: inherit;
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
            flex-wrap: wrap;
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

        /* GRID DE CONVÊNIOS E ITENS */
        .items-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
            gap: 15px;
            margin-top: 15px;
        }

        .item-card {
            border: 2px solid #e0e0e0;
            border-radius: 8px;
            padding: 15px;
            cursor: pointer;
            transition: all 0.3s;
            background: white;
        }

        .item-card:hover {
            border-color: var(--secondary);
            transform: translateY(-2px);
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.1);
        }

        .item-card.selected {
            border-color: var(--success);
            background-color: rgba(39, 174, 96, 0.05);
        }

        .item-nome {
            font-weight: 600;
            margin-bottom: 5px;
            color: var(--primary);
        }

        .item-documento {
            font-size: 0.9rem;
            color: #666;
        }

        .item-valor-input {
            margin-top: 10px;
        }

        .item-valor-input input {
            width: 100%;
            padding: 8px 12px;
            border: 1px solid #ddd;
            border-radius: 5px;
            font-size: 0.9rem;
        }

        /* LISTA DE LANÇAMENTOS */
        .lancamentos-lista {
            max-height: 300px;
            overflow-y: auto;
            border: 1px solid #e0e0e0;
            border-radius: 5px;
            margin-top: 15px;
        }

        .lancamento-item {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 10px 15px;
            border-bottom: 1px solid #f0f0f0;
            background: white;
        }

        .lancamento-item:last-child {
            border-bottom: none;
        }

        .lancamento-item:hover {
            background-color: #f8f9fa;
        }

        .lancamento-info {
            flex: 1;
        }

        .lancamento-tipo {
            font-weight: 600;
            color: var(--primary);
            font-size: 0.9rem;
        }

        .lancamento-descricao {
            font-size: 0.8rem;
            color: #666;
        }

        .lancamento-valor {
            font-weight: 600;
            color: var(--danger);
            margin-right: 10px;
        }

        .btn-remove-lancamento {
            background: none;
            border: none;
            color: var(--danger);
            cursor: pointer;
            padding: 5px;
        }

        .btn-remove-lancamento:hover {
            color: #c0392b;
        }

        /* BADGE PARA ITENS */
        .item-badge {
            display: inline-flex;
            align-items: center;
            padding: 5px 10px;
            background: #e3f2fd;
            color: #1565c0;
            border-radius: 20px;
            margin: 2px;
            font-size: 0.8rem;
        }

        .item-badge i {
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

        /* MODAIS */
        .modal {
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

        .modal-content {
            background: white;
            border-radius: 10px;
            padding: 30px;
            width: 90%;
            max-width: 600px;
            max-height: 80vh;
            overflow-y: auto;
        }

        .modal-lg {
            max-width: 800px;
        }

        /* UPLOAD DE LOGO */
        .logo-upload-area {
            border: 2px dashed var(--secondary);
            border-radius: 10px;
            padding: 20px;
            text-align: center;
            margin-bottom: 20px;
            cursor: pointer;
            transition: all 0.3s;
            background: #f8f9fa;
        }

        .logo-upload-area:hover {
            background: #e9ecef;
            border-color: var(--primary);
        }

        .logo-preview {
            max-width: 200px;
            max-height: 100px;
            margin: 10px auto;
            display: block;
            border: 1px solid #ddd;
            padding: 10px;
            border-radius: 5px;
            background: white;
        }

        .logo-preview-container {
            text-align: center;
            margin-top: 10px;
        }

        .btn-remove-logo {
            background: var(--danger);
            color: white;
            border: none;
            padding: 5px 10px;
            border-radius: 5px;
            cursor: pointer;
            font-size: 0.8rem;
            margin-top: 10px;
        }

        .btn-remove-logo:hover {
            background: #c0392b;
        }

        /* RELATÓRIO PARA IMPRESSÃO */
        @media print {
            .sidebar, .header, .btn, .tabs, .nav-menu, .form-container, .filtros-dashboard {
                display: none !important;
            }
            
            .main-content {
                padding: 0;
                margin: 0;
            }
            
            .page {
                display: block !important;
            }
            
            .table-container {
                box-shadow: none;
                padding: 0;
            }
            
            .chart-container {
                box-shadow: none;
                page-break-inside: avoid;
            }

            .relatorio-header img {
                max-width: 200px;
                margin-bottom: 10px;
            }
        }

        .relatorio-impressao {
            font-family: 'Courier New', monospace;
            padding: 20px;
        }

        .relatorio-header {
            text-align: center;
            margin-bottom: 30px;
            border-bottom: 2px solid #333;
            padding-bottom: 10px;
        }

        .relatorio-header img {
            max-width: 150px;
            max-height: 80px;
            margin-bottom: 10px;
        }

        .relatorio-footer {
            text-align: center;
            margin-top: 30px;
            font-size: 0.8rem;
            color: #666;
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

        /* UPLOAD AREA */
        .upload-area {
            border: 2px dashed #ddd;
            border-radius: 10px;
            padding: 40px;
            text-align: center;
            margin-bottom: 20px;
            transition: all 0.3s;
            cursor: pointer;
        }

        .upload-area:hover {
            border-color: var(--secondary);
        }

        .upload-icon {
            font-size: 3rem;
            color: #bbb;
            margin-bottom: 15px;
        }

        /* PROGRESS BAR */
        .progress {
            background-color: #e9ecef;
            border-radius: 0.25rem;
            overflow: hidden;
            height: 5px;
            width: 100px;
            margin-top: 5px;
        }

        .progress-bar {
            height: 100%;
            transition: width 0.6s ease;
        }

        .bg-success { background-color: #28a745 !important; }
        .bg-warning { background-color: #ffc107 !important; }
        .bg-danger { background-color: #dc3545 !important; }

        /* ACESSO RESTRITO */
        .restricted-access {
            text-align: center;
            padding: 50px;
            background: white;
            border-radius: 10px;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.05);
        }

        .restricted-access i {
            font-size: 4rem;
            color: var(--danger);
            margin-bottom: 20px;
        }

        .restricted-access h3 {
            color: var(--primary);
            margin-bottom: 10px;
        }

        .restricted-access p {
            color: #666;
        }

        /* SPINNER PEQUENO */
        .spinner-small {
            width: 20px;
            height: 20px;
            border: 3px solid rgba(0, 0, 0, 0.1);
            border-radius: 50%;
            border-top-color: var(--secondary);
            animation: spin 1s linear infinite;
            display: inline-block;
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
                <h2>AmazonDash Pro</h2>
                <p>Sistema de Gerenciamento de Dados</p>
            </div>
            
            <div id="login-alert" class="login-alert">
                <i class="fas fa-exclamation-circle"></i>
                <span></span>
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
                    <small>Versão 3.0.0 | Gestão Completa</small>
                </div>
            </form>
            
            <div class="text-center mt-4">
                <div style="margin-bottom: 15px; color: #666;">
                    <strong>Credenciais de teste:</strong>
                </div>
                <div style="background: #f8f9fa; padding: 10px; border-radius: 5px; font-size: 0.9rem;">
                    <div><strong>Administrador:</strong> admin@AmazonDash.com / admin123</div>
                    <div><strong>Gerente:</strong> gerente@empresa.com / senha123</div>
                    <div><strong>Usuário:</strong> usuario@empresa.com / senha123</div>
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
                    <div class="logo" id="sidebar-logo-container">
                        <i class="fas fa-chart-bar" id="sidebar-default-logo"></i>
                        <img id="sidebar-logo-img" class="logo-img" style="display: none;">
                        <span id="sidebar-logo-text">AmazonDash</span>
                    </div>
                </div>
                <ul class="nav-menu" id="nav-menu">
                    <!-- Itens serão carregados dinamicamente conforme permissão -->
                </ul>
            </aside>

            <!-- Conteúdo Principal -->
            <main class="main-content">
                <!-- Header -->
                <div class="header">
                    <div class="d-flex align-center">
                        <img id="header-logo-img" class="header-logo" style="display: none; margin-right: 15px;">
                        <h1 id="page-title">Dashboard</h1>
                    </div>
                    <div class="user-info">
                        <div class="user-avatar" id="user-avatar">AD</div>
                        <div class="user-details">
                            <div class="user-name" id="user-name">Carregando...</div>
                            <div class="user-role" id="user-role"></div>
                            <div class="user-centro" id="user-centro"></div>
                        </div>
                        <button id="logout-btn" class="btn btn-primary">
                            <i class="fas fa-sign-out-alt"></i> Sair
                        </button>
                    </div>
                </div>

                <!-- Páginas do Sistema -->
                <div id="dashboard" class="page active"></div>
                <div id="fechamento-diario" class="page"></div>
                <div id="convenios" class="page"></div>
                <div id="lancamentos" class="page"></div>
                <div id="centro-custo" class="page"></div>
                <div id="upload-excel" class="page"></div>
                <div id="relatorios" class="page"></div>
                <div id="usuarios" class="page"></div>
                <div id="configuracoes" class="page"></div>
            </main>
        </div>
    </div>

    <!-- Modal para Cadastro de Convênio -->
    <div id="convenio-modal" class="modal" style="display: none;">
        <div class="modal-content">
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

    <!-- Modal para Cadastro de Usuário -->
    <div id="usuario-modal" class="modal" style="display: none;">
        <div class="modal-content modal-lg">
            <h3 class="form-title" id="usuario-modal-title">Cadastrar Novo Usuário</h3>
            <form id="usuario-form">
                <input type="hidden" id="usuario-id">
                
                <div class="form-group">
                    <label class="form-label" for="usuario-nome">Nome Completo *</label>
                    <input type="text" class="form-control" id="usuario-nome" required>
                </div>
                
                <div class="form-group">
                    <label class="form-label" for="usuario-email">Email *</label>
                    <input type="email" class="form-control" id="usuario-email" required>
                </div>
                
                <div class="form-group">
                    <label class="form-label" for="usuario-senha">Senha</label>
                    <input type="password" class="form-control" id="usuario-senha" 
                           placeholder="Deixe em branco para manter a atual">
                </div>
                
                <div class="form-group">
                    <label class="form-label" for="usuario-perfil">Nível de Acesso *</label>
                    <select class="form-control" id="usuario-perfil" required>
                        <option value="">Selecione...</option>
                        <option value="Administrador">Administrador</option>
                        <option value="Gerente">Gerente</option>
                        <option value="Usuário">Usuário</option>
                    </select>
                </div>
                
                <div class="form-group">
                    <label class="form-label">Centro de Custo Vinculado *</label>
                    <select class="form-control" id="usuario-centro-custo" required>
                        <option value="">Selecione...</option>
                    </select>
                    <small class="text-muted">O usuário terá acesso apenas a este centro de custo</small>
                </div>
                
                <div class="form-group">
                    <label class="form-label" for="usuario-status">Status</label>
                    <select class="form-control" id="usuario-status">
                        <option value="Ativo">Ativo</option>
                        <option value="Inativo">Inativo</option>
                    </select>
                </div>
                
                <div class="d-flex justify-between mt-3">
                    <button type="button" id="cancel-usuario" class="btn">Cancelar</button>
                    <button type="submit" class="btn btn-primary">Salvar Usuário</button>
                </div>
            </form>
        </div>
    </div>

    <!-- Modal para Upload de Logo -->
    <div id="logo-modal" class="modal" style="display: none;">
        <div class="modal-content">
            <h3 class="form-title">Gerenciar Logomarca</h3>
            
            <div class="logo-upload-area" id="logo-drop-area">
                <div class="upload-icon">
                    <i class="fas fa-cloud-upload-alt"></i>
                </div>
                <h4>Arraste sua logo aqui ou clique para selecionar</h4>
                <p>Formatos aceitos: JPG, PNG, GIF (máx. 2MB)</p>
                <input type="file" id="logo-file-input" accept="image/*" style="display: none;">
            </div>
            
            <div id="logo-preview-container" class="logo-preview-container" style="display: none;">
                <h4>Logo atual:</h4>
                <img id="logo-preview" class="logo-preview">
                <button class="btn-remove-logo" id="remove-logo-btn">
                    <i class="fas fa-trash"></i> Remover Logo
                </button>
            </div>
            
            <div class="d-flex justify-between mt-3">
                <button type="button" id="cancel-logo" class="btn">Fechar</button>
            </div>
        </div>
    </div>

    <!-- Modal para Imprimir Relatório -->
    <div id="relatorio-modal" class="modal" style="display: none;">
        <div class="modal-content modal-lg">
            <h3 class="form-title">Visualizar/Imprimir Relatório</h3>
            <div id="relatorio-conteudo" class="relatorio-impressao">
                <!-- Conteúdo será gerado dinamicamente -->
            </div>
            <div class="d-flex justify-between mt-3">
                <button type="button" id="fechar-relatorio" class="btn">Fechar</button>
                <button type="button" id="imprimir-relatorio" class="btn btn-primary">
                    <i class="fas fa-print"></i> Imprimir
                </button>
            </div>
        </div>
    </div>

    <script>
        // ============== BANCO DE DADOS COMPLETO ==============
        const database = {
            users: [
                { 
                    id: 1, 
                    name: "Administrador", 
                    email: "admin@AmazonDash.com", 
                    password: "admin123", 
                    role: "Administrador", 
                    centroCusto: "Igapó",
                    lastLogin: new Date().toISOString(), 
                    status: "Ativo" 
                },
                { 
                    id: 2, 
                    name: "Carlos Gerente", 
                    email: "gerente@empresa.com", 
                    password: "senha123", 
                    role: "Gerente", 
                    centroCusto: "Humaitá",
                    lastLogin: "2026-01-21T10:30:00", 
                    status: "Ativo" 
                },
                { 
                    id: 3, 
                    name: "Ana Usuário", 
                    email: "usuario@empresa.com", 
                    password: "senha123", 
                    role: "Usuário", 
                    centroCusto: "Humaitá",
                    lastLogin: "2026-01-20T14:15:00", 
                    status: "Ativo" 
                }
            ],
            
            centrosCusto: [
                { codigo: "Igapó", nome: "Igapó", responsavel: "João Assis", orcamento: 50000, gasto: 32500, status: "Ativo" },
                { codigo: "Humaitá", nome: "Humaitá", responsavel: "Cleber", orcamento: 80000, gasto: 45200, status: "Ativo" },
                { codigo: "Mucuim", nome: "Mucuim", responsavel: "Diego", orcamento: 120000, gasto: 98750, status: "Ativo" },
                { codigo: "Candeias", nome: "Candeias", responsavel: "Luciana", orcamento: 75000, gasto: 42000, status: "Ativo" },
                { codigo: "Samuel", nome: "Samuel", responsavel: "Delamare", orcamento: 45000, gasto: 28000, status: "Ativo" },
                { codigo: "Machadinho", nome: "Machadinho", responsavel: "Geovane", orcamento: 60000, gasto: 38500, status: "Ativo" }
            ],
            
            convenios: [
                {
                    id: 1,
                    nome: "AMATUR",
                    documento: "12.345.678/0001-90",
                    tipo: "CNPJ",
                    centroCusto: "Humaitá",
                    status: "Ativo",
                    createdAt: "2026-01-15T10:00:00",
                    createdBy: 1
                },
                {
                    id: 2,
                    nome: "POLICIA FEDERAL",
                    documento: "98.765.432/0001-10",
                    tipo: "CNPJ",
                    centroCusto: "Machadinho",
                    status: "Ativo",
                    createdAt: "2026-01-16T14:30:00",
                    createdBy: 1
                },
                {
                    id: 3,
                    nome: "LCM",
                    documento: "123.456.789-00",
                    tipo: "CPF",
                    centroCusto: "Humaitá",
                    status: "Ativo",
                    createdAt: "2026-01-17T09:15:00",
                    createdBy: 2
                }
            ],
            
            bancos: [
                {
                    id: 1,
                    nome: "STONE",
                    agencia: "1234-5",
                    conta: "98765-4",
                    status: "Ativo",
                    createdAt: "2026-01-15T10:00:00",
                    createdBy: 1
                },
                {
                    id: 2,
                    nome: "Itaú",
                    agencia: "1234",
                    conta: "56789-0",
                    status: "Ativo",
                    createdAt: "2026-01-15T10:00:00",
                    createdBy: 1
                }
            ],
            
            tiposDespesa: [
                { id: 1, nome: "Despesa Avulsa", icone: "fa-receipt" },
                { id: 2, nome: "Abastecimento", icone: "fa-gas-pump" },
                { id: 3, nome: "Vale Adiantamento", icone: "fa-hand-holding-usd" },
                { id: 4, nome: "Material de Escritório", icone: "fa-box" },
                { id: 5, nome: "Manutenção", icone: "fa-tools" },
                { id: 6, nome: "Alimentação", icone: "fa-utensils" },
                { id: 7, nome: "Transporte", icone: "fa-bus" },
                { id: 8, nome: "Outros", icone: "fa-ellipsis-h" }
            ],
            
            fechamentosDiarios: [
                {
                    id: 1,
                    data: "2026-01-22",
                    responsavel: "Carlos Gerente",
                    centroCusto: "Humaitá",
                    dinheiro: 9220.00,
                    pix: 2450.00,
                    outrosFaturamentos: 0.00,
                    taxasBancarias: [
                        { id: 1, bancoId: 1, bancoNome: "STONE", valor: 45.50 },
                        { id: 2, bancoId: 1, bancoNome: "STONE", valor: 32.80 }
                    ],
                    despesas: [
                        { id: 1, tipo: "Abastecimento", descricao: "Posto Shell", valor: 250.00 },
                        { id: 2, tipo: "Vale Adiantamento", descricao: "João - transporte", valor: 100.00 },
                        { id: 3, tipo: "Despesa Avulsa", descricao: "Material escritório", valor: 85.50 }
                    ],
                    convenios: [
                        { id: 1, convenioId: 1, nome: "AMATUR", valor: 1500.00 },
                        { id: 2, convenioId: 3, nome: "LCM", valor: 850.00 }
                    ],
                    totalFaturamentos: 14020.00,
                    totalDespesas: 435.50,
                    totalTaxas: 78.30,
                    totalDiaria: 13506.20,
                    status: "Fechado",
                    createdAt: "2026-01-22T18:30:00",
                    createdBy: 2
                },
                {
                    id: 2,
                    data: "2026-01-23",
                    responsavel: "Ana Usuário",
                    centroCusto: "Igapó",
                    dinheiro: 5000.00,
                    pix: 3000.00,
                    outrosFaturamentos: 500.00,
                    taxasBancarias: [
                        { id: 3, bancoId: 2, bancoNome: "Itaú", valor: 25.50 }
                    ],
                    despesas: [
                        { id: 4, tipo: "Alimentação", descricao: "Almoço equipe", valor: 180.00 }
                    ],
                    convenios: [
                        { id: 3, convenioId: 2, nome: "POLICIA FEDERAL", valor: 2000.00 }
                    ],
                    totalFaturamentos: 10500.00,
                    totalDespesas: 180.00,
                    totalTaxas: 25.50,
                    totalDiaria: 10294.50,
                    status: "Fechado",
                    createdAt: "2026-01-23T19:00:00",
                    createdBy: 3
                }
            ],
            
            lancamentos: [
                { id: 1, data: "2026-01-22", descricao: "Compra de materiais", centroCusto: "Igapó", valor: 1250.50, categoria: "Despesa", createdBy: 1 }
            ],
            
            settings: {
                systemName: "AmazonDash Pro",
                currency: "BRL",
                dateFormat: "dd/mm/yyyy",
                sessionTimeout: 30,
                emailNotifications: true,
                backupAutomatico: true,
                logo: null // Armazenará a logo em base64
            }
        };

        // ============== ESTADO DA APLICAÇÃO ==============
        let currentUser = null;
        let currentPage = "dashboard";
        let sessionTimer = null;
        let isSystemLocked = true;
        let charts = {};
        let dashboardFiltros = {
            dataInicio: '',
            dataFim: '',
            centroCusto: '',
            tipo: 'todos'
        };
        
        // Estado para fechamento diário
        let selectedConvenios = [];
        let selectedTaxasBancarias = [];
        let selectedDespesas = [];
        let nextLancamentoId = 1;

        // ============== INICIALIZAÇÃO ==============
        document.addEventListener('DOMContentLoaded', function() {
            // Mostrar loading por 1.5 segundos e depois mostrar login
            setTimeout(() => {
                const loadingScreen = document.getElementById('loading-screen');
                if (loadingScreen) {
                    loadingScreen.style.display = 'none';
                }
                showLoginModal();
            }, 1500);
            
            setupLogin();
            
            const logoutBtn = document.getElementById('logout-btn');
            if (logoutBtn) {
                logoutBtn.addEventListener('click', function(e) {
                    e.preventDefault();
                    logoutUser();
                });
            }
            
            // Configurar modais
            const cancelConvenio = document.getElementById('cancel-convenio');
            if (cancelConvenio) {
                cancelConvenio.addEventListener('click', function() {
                    document.getElementById('convenio-modal').style.display = 'none';
                });
            }
            
            const convenioForm = document.getElementById('convenio-form');
            if (convenioForm) {
                convenioForm.addEventListener('submit', function(e) {
                    e.preventDefault();
                    saveConvenio();
                });
            }
            
            const cancelUsuario = document.getElementById('cancel-usuario');
            if (cancelUsuario) {
                cancelUsuario.addEventListener('click', function() {
                    document.getElementById('usuario-modal').style.display = 'none';
                });
            }
            
            const usuarioForm = document.getElementById('usuario-form');
            if (usuarioForm) {
                usuarioForm.addEventListener('submit', function(e) {
                    e.preventDefault();
                    saveUsuario();
                });
            }
            
            const fecharRelatorio = document.getElementById('fechar-relatorio');
            if (fecharRelatorio) {
                fecharRelatorio.addEventListener('click', function() {
                    document.getElementById('relatorio-modal').style.display = 'none';
                });
            }
            
            const imprimirRelatorio = document.getElementById('imprimir-relatorio');
            if (imprimirRelatorio) {
                imprimirRelatorio.addEventListener('click', function() {
                    window.print();
                });
            }

            // Configurar upload de logo
            setupLogoUpload();
        });

        // ============== SISTEMA DE LOGO ==============
        function setupLogoUpload() {
            const logoModal = document.getElementById('logo-modal');
            const logoDropArea = document.getElementById('logo-drop-area');
            const logoFileInput = document.getElementById('logo-file-input');
            const cancelLogo = document.getElementById('cancel-logo');
            const removeLogoBtn = document.getElementById('remove-logo-btn');

            if (logoDropArea) {
                logoDropArea.addEventListener('click', function() {
                    logoFileInput.click();
                });

                ['dragenter', 'dragover', 'dragleave', 'drop'].forEach(eventName => {
                    logoDropArea.addEventListener(eventName, preventDefaults, false);
                });

                ['dragenter', 'dragover'].forEach(eventName => {
                    logoDropArea.addEventListener(eventName, highlight, false);
                });

                ['dragleave', 'drop'].forEach(eventName => {
                    logoDropArea.addEventListener(eventName, unhighlight, false);
                });

                logoDropArea.addEventListener('drop', handleDrop, false);

                function preventDefaults(e) {
                    e.preventDefault();
                    e.stopPropagation();
                }

                function highlight() {
                    logoDropArea.style.borderColor = 'var(--primary)';
                    logoDropArea.style.backgroundColor = '#e9ecef';
                }

                function unhighlight() {
                    logoDropArea.style.borderColor = 'var(--secondary)';
                    logoDropArea.style.backgroundColor = '#f8f9fa';
                }

                function handleDrop(e) {
                    const dt = e.dataTransfer;
                    const files = dt.files;
                    
                    if (files.length) {
                        handleLogoFile(files[0]);
                    }
                }
            }

            if (logoFileInput) {
                logoFileInput.addEventListener('change', function(e) {
                    if (this.files.length) {
                        handleLogoFile(this.files[0]);
                    }
                });
            }

            if (cancelLogo) {
                cancelLogo.addEventListener('click', function() {
                    logoModal.style.display = 'none';
                });
            }

            if (removeLogoBtn) {
                removeLogoBtn.addEventListener('click', function() {
                    removeLogo();
                });
            }

            // Adicionar item de menu para configurações de logo (apenas para admin)
            const navMenu = document.getElementById('nav-menu');
            if (navMenu && currentUser && currentUser.role === 'Administrador') {
                const logoMenuItem = document.createElement('li');
                logoMenuItem.className = 'nav-item';
                logoMenuItem.innerHTML = `
                    <a class="nav-link" id="nav-logo">
                        <i class="fas fa-image"></i>
                        <span class="nav-text">Logomarca</span>
                    </a>
                `;
                navMenu.appendChild(logoMenuItem);

                document.getElementById('nav-logo').addEventListener('click', function() {
                    showLogoModal();
                });
            }
        }

        function handleLogoFile(file) {
            if (!file.type.match('image.*')) {
                showAlert('Por favor, selecione apenas arquivos de imagem.', 'warning');
                return;
            }

            if (file.size > 2 * 1024 * 1024) {
                showAlert('A imagem não pode ter mais que 2MB.', 'warning');
                return;
            }

            const reader = new FileReader();
            reader.onload = function(e) {
                database.settings.logo = e.target.result;
                updateLogoDisplay();
                
                const previewContainer = document.getElementById('logo-preview-container');
                const preview = document.getElementById('logo-preview');
                if (previewContainer && preview) {
                    preview.src = e.target.result;
                    previewContainer.style.display = 'block';
                }
                
                showAlert('Logo atualizada com sucesso!', 'success');
            };
            reader.readAsDataURL(file);
        }

        function removeLogo() {
            if (confirm('Tem certeza que deseja remover a logomarca?')) {
                database.settings.logo = null;
                updateLogoDisplay();
                
                const previewContainer = document.getElementById('logo-preview-container');
                if (previewContainer) {
                    previewContainer.style.display = 'none';
                }
                
                showAlert('Logo removida com sucesso!', 'success');
            }
        }

        function updateLogoDisplay() {
            const sidebarLogoImg = document.getElementById('sidebar-logo-img');
            const sidebarDefaultLogo = document.getElementById('sidebar-default-logo');
            const sidebarLogoText = document.getElementById('sidebar-logo-text');
            const headerLogoImg = document.getElementById('header-logo-img');

            if (database.settings.logo) {
                // Sidebar
                if (sidebarLogoImg) {
                    sidebarLogoImg.src = database.settings.logo;
                    sidebarLogoImg.style.display = 'inline-block';
                }
                if (sidebarDefaultLogo) sidebarDefaultLogo.style.display = 'none';
                if (sidebarLogoText) sidebarLogoText.style.display = 'none';

                // Header
                if (headerLogoImg) {
                    headerLogoImg.src = database.settings.logo;
                    headerLogoImg.style.display = 'inline-block';
                }
            } else {
                // Sidebar
                if (sidebarLogoImg) sidebarLogoImg.style.display = 'none';
                if (sidebarDefaultLogo) sidebarDefaultLogo.style.display = 'inline-block';
                if (sidebarLogoText) sidebarLogoText.style.display = 'inline-block';

                // Header
                if (headerLogoImg) headerLogoImg.style.display = 'none';
            }
        }

        function showLogoModal() {
            const modal = document.getElementById('logo-modal');
            const previewContainer = document.getElementById('logo-preview-container');
            const preview = document.getElementById('logo-preview');

            if (database.settings.logo) {
                preview.src = database.settings.logo;
                previewContainer.style.display = 'block';
            } else {
                previewContainer.style.display = 'none';
            }

            modal.style.display = 'flex';
        }

        // ============== SISTEMA DE LOGIN ==============
        function setupLogin() {
            const loginForm = document.getElementById('login-form');
            const loginAlert = document.getElementById('login-alert');
            
            if (!loginForm) return;
            
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
            
            const emailInput = document.getElementById('login-email');
            const passwordInput = document.getElementById('login-password');
            
            if (emailInput) {
                emailInput.addEventListener('input', function() {
                    if (loginAlert) loginAlert.style.display = 'none';
                });
            }
            
            if (passwordInput) {
                passwordInput.addEventListener('input', function() {
                    if (loginAlert) loginAlert.style.display = 'none';
                });
            }
        }
        
        function showLoginError(message) {
            const loginAlert = document.getElementById('login-alert');
            if (!loginAlert) return;
            
            const span = loginAlert.querySelector('span');
            if (span) span.textContent = message;
            loginAlert.style.display = 'block';
        }
        
        function loginUser(user) {
            currentUser = user;
            isSystemLocked = false;
            
            const loginModal = document.getElementById('login-modal');
            const mainSystem = document.getElementById('main-system');
            
            if (loginModal) loginModal.style.display = 'none';
            if (mainSystem) mainSystem.style.display = 'flex';
            
            updateUserInfo();
            buildNavigation();
            startSessionTimer();
            updateLogoDisplay();
            
            // Carregar página inicial
            showPage('dashboard');
            
            showAlert(`Bem-vindo(a), ${user.name}!`, 'success');
        }
        
        function logoutUser() {
            if (confirm('Tem certeza que deseja sair do sistema?')) {
                if (sessionTimer) {
                    clearTimeout(sessionTimer);
                    sessionTimer = null;
                }
                
                currentUser = null;
                isSystemLocked = true;
                
                const mainSystem = document.getElementById('main-system');
                if (mainSystem) mainSystem.style.display = 'none';
                
                showLoginModal();
                
                const loginForm = document.getElementById('login-form');
                if (loginForm) loginForm.reset();
                
                const loginAlert = document.getElementById('login-alert');
                if (loginAlert) loginAlert.style.display = 'none';
                
                showAlert('Você saiu do sistema com sucesso.', 'info');
            }
        }
        
        function showLoginModal() {
            const loginModal = document.getElementById('login-modal');
            if (loginModal) {
                loginModal.style.display = 'flex';
            }
        }
        
        function updateUserInfo() {
            if (!currentUser) return;
            
            const userName = document.getElementById('user-name');
            const userRole = document.getElementById('user-role');
            const userAvatar = document.getElementById('user-avatar');
            const userCentro = document.getElementById('user-centro');
            
            if (userName) userName.textContent = currentUser.name;
            if (userRole) userRole.textContent = currentUser.role;
            if (userAvatar) {
                userAvatar.textContent = currentUser.name.split(' ').map(n => n[0]).join('').substring(0, 2).toUpperCase();
            }
            
            // Mostrar centro de custo vinculado
            const centro = database.centrosCusto.find(c => c.codigo === currentUser.centroCusto);
            if (userCentro) {
                userCentro.textContent = centro ? `${centro.codigo} - ${centro.nome}` : 'Sem centro vinculado';
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
            
            const resetTimer = () => {
                if (sessionTimer) {
                    clearTimeout(sessionTimer);
                    sessionTimer = setTimeout(() => {
                        showSessionExpired();
                    }, timeoutMs);
                }
            };
            
            ['click', 'mousemove', 'keypress'].forEach(event => {
                document.removeEventListener(event, resetTimer);
                document.addEventListener(event, resetTimer);
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
            
            const reloadBtn = document.getElementById('reload-session');
            if (reloadBtn) {
                reloadBtn.addEventListener('click', function() {
                    if (overlay.parentNode) {
                        document.body.removeChild(overlay);
                    }
                    logoutUser();
                });
            }
            
            setTimeout(() => {
                if (overlay.parentNode) {
                    document.body.removeChild(overlay);
                    logoutUser();
                }
            }, 5000);
        }

        // ============== CONSTRUÇÃO DA NAVEGAÇÃO ==============
        function buildNavigation() {
            const navMenu = document.getElementById('nav-menu');
            if (!navMenu || !currentUser) return;
            
            // Definir itens de menu baseados no perfil
            const menuItems = [
                { page: 'dashboard', icon: 'fa-tachometer-alt', text: 'Dashboard', roles: ['Administrador', 'Gerente', 'Usuário'] },
                { page: 'fechamento-diario', icon: 'fa-calendar-day', text: 'Fechamento Diário', roles: ['Administrador', 'Gerente', 'Usuário'] },
                { page: 'convenios', icon: 'fa-handshake', text: 'Convênios', roles: ['Administrador', 'Gerente'] },
                { page: 'lancamentos', icon: 'fa-file-import', text: 'Lançamentos', roles: ['Administrador', 'Gerente', 'Usuário'] },
                { page: 'centro-custo', icon: 'fa-tags', text: 'Centros de Custo', roles: ['Administrador', 'Gerente'] },
                { page: 'upload-excel', icon: 'fa-file-excel', text: 'Importar Excel', roles: ['Administrador', 'Gerente'] },
                { page: 'relatorios', icon: 'fa-chart-pie', text: 'Relatórios', roles: ['Administrador', 'Gerente'] },
                { page: 'usuarios', icon: 'fa-users', text: 'Usuários', roles: ['Administrador'] },
                { page: 'configuracoes', icon: 'fa-cog', text: 'Configurações', roles: ['Administrador'] }
            ];
            
            // Filtrar itens baseado no perfil do usuário
            const allowedItems = menuItems.filter(item => 
                item.roles.includes(currentUser.role)
            );
            
            let html = '';
            allowedItems.forEach(item => {
                html += `
                    <li class="nav-item">
                        <a class="nav-link ${item.page === currentPage ? 'active' : ''}" data-page="${item.page}">
                            <i class="fas ${item.icon}"></i>
                            <span class="nav-text">${item.text}</span>
                        </a>
                    </li>
                `;
            });

            // Adicionar item de logo para administradores
            if (currentUser.role === 'Administrador') {
                html += `
                    <li class="nav-item">
                        <a class="nav-link" id="nav-logo">
                            <i class="fas fa-image"></i>
                            <span class="nav-text">Logomarca</span>
                        </a>
                    </li>
                `;
            }
            
            navMenu.innerHTML = html;
            
            // Adicionar eventos de clique
            document.querySelectorAll('.nav-link').forEach(link => {
                if (link.id === 'nav-logo') {
                    link.addEventListener('click', function(e) {
                        e.preventDefault();
                        showLogoModal();
                    });
                } else {
                    link.addEventListener('click', function(e) {
                        e.preventDefault();
                        
                        const pageId = this.getAttribute('data-page');
                        
                        document.querySelectorAll('.nav-link').forEach(item => {
                            item.classList.remove('active');
                        });
                        this.classList.add('active');
                        
                        showPage(pageId);
                        
                        const pageTitle = document.getElementById('page-title');
                        const navText = this.querySelector('.nav-text');
                        if (pageTitle && navText) {
                            pageTitle.textContent = navText.textContent;
                        }
                    });
                }
            });
        }
        
        function showPage(pageId) {
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

        // ============== FUNÇÕES UTILITÁRIAS ==============
        function getCentrosCustoOptions() {
            if (!currentUser) return '';
            
            let centros = database.centrosCusto;
            
            // Se não for admin, mostrar apenas centros permitidos
            if (currentUser.role !== "Administrador") {
                centros = centros.filter(c => c.codigo === currentUser.centroCusto);
            }
            
            return centros.map(c => `<option value="${c.codigo}">${c.codigo} - ${c.nome}</option>`).join('');
        }
        
        function getCentrosCustoForUser() {
            if (!currentUser) return [];
            
            if (currentUser.role === "Administrador") {
                return database.centrosCusto;
            } else {
                return database.centrosCusto.filter(c => c.codigo === currentUser.centroCusto);
            }
        }
        
        function filterByUserCentro(items, centroField = 'centroCusto') {
            if (!currentUser) return items;
            
            if (currentUser.role === "Administrador") {
                return items;
            } else {
                return items.filter(item => item[centroField] === currentUser.centroCusto);
            }
        }
        
        function filterDataByDateRange(items, dataInicio, dataFim) {
            if (!dataInicio && !dataFim) return items;
            
            return items.filter(item => {
                const itemDate = new Date(item.data);
                
                if (dataInicio && dataFim) {
                    return itemDate >= new Date(dataInicio) && itemDate <= new Date(dataFim);
                } else if (dataInicio) {
                    return itemDate >= new Date(dataInicio);
                } else if (dataFim) {
                    return itemDate <= new Date(dataFim);
                }
                
                return true;
            });
        }
        
        function filterDataByTipo(items, tipo) {
            if (!tipo || tipo === 'todos') return items;
            
            switch(tipo) {
                case 'fechamentos':
                    return items.filter(item => item.totalDiaria !== undefined);
                case 'convenios':
                    return items.filter(item => item.convenios && item.convenios.length > 0);
                case 'despesas':
                    return items.filter(item => item.despesas && item.despesas.length > 0);
                case 'taxas':
                    return items.filter(item => item.taxasBancarias && item.taxasBancarias.length > 0);
                default:
                    return items;
            }
        }
        
        function aplicarFiltrosDashboard() {
            if (!dashboardFiltros) return;
            
            // Obter valores dos filtros
            const dataInicio = document.getElementById('filtro-data-inicio')?.value || dashboardFiltros.dataInicio;
            const dataFim = document.getElementById('filtro-data-fim')?.value || dashboardFiltros.dataFim;
            const centroCusto = document.getElementById('filtro-centro-custo')?.value || dashboardFiltros.centroCusto;
            const tipo = document.getElementById('filtro-tipo')?.value || dashboardFiltros.tipo;
            
            // Atualizar objeto de filtros
            dashboardFiltros = {
                dataInicio,
                dataFim,
                centroCusto,
                tipo
            };
            
            // Atualizar UI dos filtros ativos
            atualizarFiltrosAtivos();
            
            // Atualizar dashboard
            atualizarDashboardComFiltros();
        }
        
        function limparFiltrosDashboard() {
            // Resetar campos
            const dataInicio = document.getElementById('filtro-data-inicio');
            const dataFim = document.getElementById('filtro-data-fim');
            const centroCusto = document.getElementById('filtro-centro-custo');
            const tipo = document.getElementById('filtro-tipo');
            
            if (dataInicio) dataInicio.value = '';
            if (dataFim) dataFim.value = '';
            if (centroCusto) centroCusto.value = '';
            if (tipo) tipo.value = 'todos';
            
            // Resetar objeto de filtros
            dashboardFiltros = {
                dataInicio: '',
                dataFim: '',
                centroCusto: '',
                tipo: 'todos'
            };
            
            // Atualizar UI
            atualizarFiltrosAtivos();
            
            // Atualizar dashboard
            atualizarDashboardComFiltros();
        }
        
        function removerFiltro(tipo) {
            switch(tipo) {
                case 'data':
                    const dataInicio = document.getElementById('filtro-data-inicio');
                    const dataFim = document.getElementById('filtro-data-fim');
                    if (dataInicio) dataInicio.value = '';
                    if (dataFim) dataFim.value = '';
                    dashboardFiltros.dataInicio = '';
                    dashboardFiltros.dataFim = '';
                    break;
                case 'centro':
                    const centroCusto = document.getElementById('filtro-centro-custo');
                    if (centroCusto) centroCusto.value = '';
                    dashboardFiltros.centroCusto = '';
                    break;
                case 'tipo':
                    const tipo = document.getElementById('filtro-tipo');
                    if (tipo) tipo.value = 'todos';
                    dashboardFiltros.tipo = 'todos';
                    break;
            }
            
            atualizarFiltrosAtivos();
            atualizarDashboardComFiltros();
        }
        
        function atualizarFiltrosAtivos() {
            const container = document.getElementById('filtros-ativos');
            if (!container) return;
            
            let html = '';
            
            if (dashboardFiltros.dataInicio || dashboardFiltros.dataFim) {
                let periodoText = 'Período: ';
                if (dashboardFiltros.dataInicio) periodoText += formatDate(dashboardFiltros.dataInicio);
                if (dashboardFiltros.dataInicio && dashboardFiltros.dataFim) periodoText += ' até ';
                if (dashboardFiltros.dataFim) periodoText += formatDate(dashboardFiltros.dataFim);
                
                html += `
                    <span class="filtro-tag">
                        ${periodoText}
                        <i class="fas fa-times" onclick="window.removerFiltro('data')"></i>
                    </span>
                `;
            }
            
            if (dashboardFiltros.centroCusto) {
                const centro = database.centrosCusto.find(c => c.codigo === dashboardFiltros.centroCusto);
                html += `
                    <span class="filtro-tag">
                        Centro: ${centro ? centro.nome : dashboardFiltros.centroCusto}
                        <i class="fas fa-times" onclick="window.removerFiltro('centro')"></i>
                    </span>
                `;
            }
            
            if (dashboardFiltros.tipo && dashboardFiltros.tipo !== 'todos') {
                const tipoText = {
                    'fechamentos': 'Fechamentos',
                    'convenios': 'Convênios',
                    'despesas': 'Despesas',
                    'taxas': 'Taxas'
                }[dashboardFiltros.tipo] || 'Todos';
                
                html += `
                    <span class="filtro-tag">
                        Tipo: ${tipoText}
                        <i class="fas fa-times" onclick="window.removerFiltro('tipo')"></i>
                    </span>
                `;
            }
            
            container.innerHTML = html || '<span class="text-muted">Nenhum filtro aplicado</span>';
        }
        
        function formatDate(dateString) {
            try {
                const date = new Date(dateString);
                if (isNaN(date.getTime())) return 'Data inválida';
                
                return date.toLocaleDateString('pt-BR');
            } catch (e) {
                return dateString;
            }
        }
        
        function formatCurrency(value) {
            return new Intl.NumberFormat('pt-BR', {
                style: 'currency',
                currency: 'BRL'
            }).format(value || 0);
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
                <button class="close-alert">
                    <i class="fas fa-times"></i>
                </button>
            `;
            
            const mainContent = document.querySelector('.main-content');
            if (mainContent) {
                mainContent.insertBefore(alert, mainContent.firstChild);
                
                const closeBtn = alert.querySelector('.close-alert');
                if (closeBtn) {
                    closeBtn.addEventListener('click', function() {
                        alert.remove();
                    });
                }
                
                setTimeout(() => {
                    if (alert.parentNode) {
                        alert.remove();
                    }
                }, 5000);
            }
        }

        // ============== DASHBOARD COM FILTROS ==============
        function loadDashboardContent() {
            const dashboardPage = document.getElementById('dashboard');
            if (!dashboardPage || !currentUser) return;
            
            const logoHTML = database.settings.logo ? 
                `<img src="${database.settings.logo}" class="dashboard-welcome-logo">` : '';

            dashboardPage.innerHTML = `
                <div class="dashboard-welcome">
                    ${logoHTML}
                    <h2>Bem-vindo ao ${database.settings.systemName}</h2>
                    <p>Olá, ${currentUser ? currentUser.name : 'Usuário'}! Aqui está o resumo do seu sistema.</p>
                </div>
                
                <!-- FILTROS DO DASHBOARD -->
                <div class="filtros-dashboard">
                    <h3 style="margin-bottom: 15px; color: var(--primary);">
                        <i class="fas fa-filter"></i> Filtros
                    </h3>
                    
                    <div class="filtros-grid">
                        <div class="filtro-group">
                            <label class="filtro-label">
                                <i class="fas fa-calendar-alt"></i> Data Início
                            </label>
                            <input type="date" class="filtro-input" id="filtro-data-inicio" 
                                   value="${dashboardFiltros.dataInicio}">
                        </div>
                        
                        <div class="filtro-group">
                            <label class="filtro-label">
                                <i class="fas fa-calendar-alt"></i> Data Fim
                            </label>
                            <input type="date" class="filtro-input" id="filtro-data-fim" 
                                   value="${dashboardFiltros.dataFim}">
                        </div>
                        
                        <div class="filtro-group">
                            <label class="filtro-label">
                                <i class="fas fa-tags"></i> Centro de Custo
                            </label>
                            <select class="filtro-input" id="filtro-centro-custo">
                                <option value="">Todos</option>
                                ${getCentrosCustoOptions()}
                            </select>
                        </div>
                        
                        <div class="filtro-group">
                            <label class="filtro-label">
                                <i class="fas fa-chart-pie"></i> Tipo
                            </label>
                            <select class="filtro-input" id="filtro-tipo">
                                <option value="todos">Todos</option>
                                <option value="fechamentos">Fechamentos</option>
                                <option value="convenios">Convênios</option>
                                <option value="despesas">Despesas</option>
                                <option value="taxas">Taxas</option>
                            </select>
                        </div>
                    </div>
                    
                    <div class="filtros-actions">
                        <button class="btn btn-sm" onclick="window.limparFiltrosDashboard()">
                            <i class="fas fa-eraser"></i> Limpar
                        </button>
                        <button class="btn btn-sm btn-primary" onclick="window.aplicarFiltrosDashboard()">
                            <i class="fas fa-search"></i> Aplicar Filtros
                        </button>
                    </div>
                    
                    <div class="filtros-ativos" id="filtros-ativos">
                        <!-- Filtros ativos serão carregados aqui -->
                    </div>
                </div>
                
                <!-- CARDS -->
                <div class="cards-container">
                    <div class="card">
                        <div class="card-title">Fechamentos do Mês</div>
                        <div class="card-value" id="total-fechamentos">0</div>
                        <div class="card-footer">
                            <span class="info-text" id="fechamentos-info">-</span>
                        </div>
                    </div>
                    <div class="card">
                        <div class="card-title">Total em Convênios</div>
                        <div class="card-value" id="total-convenios">R$ 0,00</div>
                        <div class="card-footer">
                            <span class="success-text" id="convenios-ativos">-</span> ativos
                        </div>
                    </div>
                    <div class="card">
                        <div class="card-title">Despesas do Dia</div>
                        <div class="card-value" id="total-despesas">R$ 0,00</div>
                        <div class="card-footer">
                            <span class="warning-text" id="despesas-info">-</span>
                        </div>
                    </div>
                    <div class="card">
                        <div class="card-title">Taxas Bancárias</div>
                        <div class="card-value" id="total-taxas">R$ 0,00</div>
                        <div class="card-footer">
                            <span class="danger-text" id="taxas-info">-</span> este mês
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
                                <button class="btn btn-primary btn-sm" id="ver-todos-fechamentos">
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
                                <button class="btn btn-primary btn-sm" id="ver-todos-convenios">
                                    <i class="fas fa-handshake"></i> Ver Todos
                                </button>
                            </div>
                        </div>
                    </div>
                </div>
            `;
            
            // Configurar eventos
            const verTodosFechamentos = document.getElementById('ver-todos-fechamentos');
            if (verTodosFechamentos) {
                verTodosFechamentos.addEventListener('click', function() {
                    showPage('fechamento-diario');
                });
            }
            
            const verTodosConvenios = document.getElementById('ver-todos-convenios');
            if (verTodosConvenios) {
                verTodosConvenios.addEventListener('click', function() {
                    showPage('convenios');
                });
            }
            
            // Atualizar filtros ativos
            atualizarFiltrosAtivos();
            
            // Atualizar dashboard
            atualizarDashboardComFiltros();
        }
        
        function atualizarDashboardComFiltros() {
            // Mostrar indicador de carregamento nos cards
            const cards = document.querySelectorAll('.card-value');
            cards.forEach(card => {
                if (card.id !== 'convenios-ativos' && card.id !== 'fechamentos-info' && 
                    card.id !== 'despesas-info' && card.id !== 'taxas-info') {
                    card.innerHTML = '<div class="spinner-small"></div>';
                }
            });
            
            setTimeout(() => {
                // Filtrar dados
                let fechamentosFiltrados = filterByUserCentro(database.fechamentosDiarios);
                
                // Aplicar filtro de centro de custo
                if (dashboardFiltros.centroCusto) {
                    fechamentosFiltrados = fechamentosFiltrados.filter(f => f.centroCusto === dashboardFiltros.centroCusto);
                }
                
                // Aplicar filtro de data
                fechamentosFiltrados = filterDataByDateRange(fechamentosFiltrados, dashboardFiltros.dataInicio, dashboardFiltros.dataFim);
                
                // Aplicar filtro de tipo
                fechamentosFiltrados = filterDataByTipo(fechamentosFiltrados, dashboardFiltros.tipo);
                
                // Calcular métricas
                const metricas = calcularMetricas(fechamentosFiltrados);
                
                // Atualizar cards
                atualizarCardsDashboard(metricas);
                
                // Atualizar gráficos
                setupDashboardCharts(fechamentosFiltrados);
                
                // Atualizar listas
                atualizarListasDashboard(fechamentosFiltrados);
            }, 300);
        }
        
        function calcularMetricas(fechamentos) {
            const hoje = new Date();
            const mesAtual = hoje.getMonth() + 1;
            const anoAtual = hoje.getFullYear();
            
            let totalFechamentos = fechamentos.length;
            let totalConvenios = 0;
            let totalDespesas = 0;
            let totalTaxas = 0;
            
            fechamentos.forEach(f => {
                if (f.convenios) {
                    totalConvenios += f.convenios.reduce((sum, c) => sum + c.valor, 0);
                }
                if (f.despesas) {
                    totalDespesas += f.despesas.reduce((sum, d) => sum + d.valor, 0);
                }
                if (f.taxasBancarias) {
                    totalTaxas += f.taxasBancarias.reduce((sum, t) => sum + t.valor, 0);
                }
            });
            
            const fechamentosMesAtual = fechamentos.filter(f => {
                const data = new Date(f.data);
                return data.getMonth() + 1 === mesAtual && data.getFullYear() === anoAtual;
            });
            
            const totalFechamentosMes = fechamentosMesAtual.length;
            const conveniosAtivos = filterByUserCentro(database.convenios.filter(c => c.status === 'Ativo')).length;
            
            return {
                totalFechamentosMes,
                totalConvenios,
                totalDespesas,
                totalTaxas,
                conveniosAtivos,
                totalFechamentos
            };
        }
        
        function atualizarCardsDashboard(metricas) {
            const totalFechamentos = document.getElementById('total-fechamentos');
            const totalConvenios = document.getElementById('total-convenios');
            const totalDespesas = document.getElementById('total-despesas');
            const totalTaxas = document.getElementById('total-taxas');
            const conveniosAtivos = document.getElementById('convenios-ativos');
            
            if (totalFechamentos) totalFechamentos.textContent = metricas.totalFechamentosMes;
            if (totalConvenios) totalConvenios.textContent = formatCurrency(metricas.totalConvenios);
            if (totalDespesas) totalDespesas.textContent = formatCurrency(metricas.totalDespesas);
            if (totalTaxas) totalTaxas.textContent = formatCurrency(metricas.totalTaxas);
            if (conveniosAtivos) conveniosAtivos.textContent = metricas.conveniosAtivos;
        }
        
        function setupDashboardCharts(fechamentos) {
            // Destruir gráficos existentes
            if (charts.faturamentoChart) {
                charts.faturamentoChart.destroy();
                charts.faturamentoChart = null;
            }
            if (charts.conveniosChart) {
                charts.conveniosChart.destroy();
                charts.conveniosChart = null;
            }
            
            // Calcular totais
            let totalDinheiro = 0, totalPix = 0, totalConvenios = 0, totalOutros = 0;
            
            fechamentos.forEach(f => {
                totalDinheiro += f.dinheiro || 0;
                totalPix += f.pix || 0;
                totalOutros += f.outrosFaturamentos || 0;
                
                if (f.convenios) {
                    totalConvenios += f.convenios.reduce((sum, c) => sum + c.valor, 0);
                }
            });
            
            // Gráfico de faturamento
            const faturamentoCtx = document.getElementById('faturamentoChart');
            if (faturamentoCtx) {
                try {
                    charts.faturamentoChart = new Chart(faturamentoCtx.getContext('2d'), {
                        type: 'doughnut',
                        data: {
                            labels: ['Dinheiro', 'PIX', 'Convênios', 'Outros'],
                            datasets: [{
                                data: [totalDinheiro, totalPix, totalConvenios, totalOutros],
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
                } catch (e) {
                    console.log('Erro ao criar gráfico de faturamento:', e);
                }
            }
            
            // Gráfico de convênios
            const conveniosAtivos = filterByUserCentro(database.convenios.filter(c => c.status === 'Ativo'));
            const conveniosNomes = conveniosAtivos.map(c => c.nome);
            const conveniosValores = conveniosAtivos.map(() => Math.random() * 2000 + 500);
            
            const conveniosCtx = document.getElementById('conveniosChart');
            if (conveniosCtx) {
                try {
                    charts.conveniosChart = new Chart(conveniosCtx.getContext('2d'), {
                        type: 'bar',
                        data: {
                            labels: conveniosNomes,
                            datasets: [{
                                label: 'Valor (R$)',
                                data: conveniosValores,
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
                } catch (e) {
                    console.log('Erro ao criar gráfico de convênios:', e);
                }
            }
        }
        
        function atualizarListasDashboard(fechamentos) {
            // Últimos fechamentos
            const ultimosFechamentos = fechamentos
                .sort((a, b) => new Date(b.data) - new Date(a.data))
                .slice(0, 3);
            
            const fechamentosHTML = ultimosFechamentos.map(f => {
                const totalConvenios = f.convenios ? f.convenios.reduce((sum, c) => sum + c.valor, 0) : 0;
                return `
                    <div class="summary-item">
                        <span class="summary-label">${formatDate(f.data)}</span>
                        <span class="summary-value">${formatCurrency(f.totalDiaria || 0)}</span>
                    </div>
                    <div class="summary-item" style="font-size: 0.8rem; color: #666; padding-top: 0;">
                        Convênios: ${formatCurrency(totalConvenios)}
                    </div>
                `;
            }).join('');
            
            const ultimosFechamentosEl = document.getElementById('ultimos-fechamentos');
            if (ultimosFechamentosEl) {
                ultimosFechamentosEl.innerHTML = fechamentosHTML || 
                    '<div class="text-center">Nenhum fechamento encontrado</div>';
            }
            
            // Últimos convênios
            const ultimosConvenios = filterByUserCentro(database.convenios)
                .filter(c => c.status === 'Ativo')
                .slice(0, 3);
            
            const conveniosHTML = ultimosConvenios.map(c => `
                <div class="summary-item">
                    <span class="summary-label">${c.nome}</span>
                    <span class="summary-value">${c.documento}</span>
                </div>
            `).join('');
            
            const ultimosConveniosEl = document.getElementById('ultimos-convenios');
            if (ultimosConveniosEl) {
                ultimosConveniosEl.innerHTML = conveniosHTML || 
                    '<div class="text-center">Nenhum convênio encontrado</div>';
            }
        }

        // ============== FUNÇÃO DE IMPRESSÃO DO RELATÓRIO (MODIFICADA) ==============
        function imprimirRelatorioFechamento(fechamento) {
            const centro = database.centrosCusto.find(c => c.codigo === fechamento.centroCusto);
            
            const logoHTML = database.settings.logo ? 
                `<img src="${database.settings.logo}" style="max-width: 150px; max-height: 80px; margin-bottom: 10px;">` : '';

            let conveniosHTML = '';
            if (fechamento.convenios && fechamento.convenios.length > 0) {
                fechamento.convenios.forEach(c => {
                    conveniosHTML += `
                        <tr>
                            <td>${c.nome}</td>
                            <td class="text-right">${formatCurrency(c.valor)}</td>
                        </tr>
                    `;
                });
            } else {
                conveniosHTML = '<tr><td colspan="2" class="text-center">Nenhum convênio</td></tr>';
            }
            
            let despesasHTML = '';
            if (fechamento.despesas && fechamento.despesas.length > 0) {
                fechamento.despesas.forEach(d => {
                    despesasHTML += `
                        <tr>
                            <td>${d.tipo}</td>
                            <td>${d.descricao}</td>
                            <td class="text-right">${formatCurrency(d.valor)}</td>
                        </tr>
                    `;
                });
            } else {
                despesasHTML = '<tr><td colspan="3" class="text-center">Nenhuma despesa</td></tr>';
            }
            
            const relatorioHTML = `
                <div class="relatorio-header">
                    ${logoHTML}
                    <h2>${database.settings.systemName}</h2>
                    <h3>Relatório de Fechamento Diário</h3>
                    <p>Data do fechamento: ${formatDate(fechamento.data)}</p>
                </div>
                
                <div style="margin-bottom: 20px;">
                    <p><strong>Responsável:</strong> ${fechamento.responsavel}</p>
                    <p><strong>Centro de Custo:</strong> ${centro ? centro.nome : fechamento.centroCusto}</p>
                    <p><strong>Data de emissão:</strong> ${new Date().toLocaleString('pt-BR')}</p>
                </div>
                
                <h4>Faturamento</h4>
                <table style="width: 100%; border-collapse: collapse; margin-bottom: 20px;">
                    <tr style="border-bottom: 1px solid #ddd;">
                        <th style="text-align: left;">Tipo</th>
                        <th style="text-align: right;">Valor</th>
                    </tr>
                    <tr>
                        <td>Dinheiro</td>
                        <td class="text-right">${formatCurrency(fechamento.dinheiro)}</td>
                    </tr>
                    <tr>
                        <td>PIX</td>
                        <td class="text-right">${formatCurrency(fechamento.pix)}</td>
                    </tr>
                    <tr>
                        <td>Outros Faturamentos</td>
                        <td class="text-right">${formatCurrency(fechamento.outrosFaturamentos || 0)}</td>
                    </tr>
                    ${conveniosHTML}
                    <tr style="border-top: 2px solid #333; font-weight: bold;">
                        <td>Total Faturamentos</td>
                        <td class="text-right">${formatCurrency(fechamento.totalFaturamentos)}</td>
                    </tr>
                </table>
                
                <h4>Despesas do Dia</h4>
                <table style="width: 100%; border-collapse: collapse; margin-bottom: 20px;">
                    <tr style="border-bottom: 1px solid #ddd;">
                        <th style="text-align: left;">Tipo</th>
                        <th style="text-align: left;">Descrição</th>
                        <th style="text-align: right;">Valor</th>
                    </tr>
                    ${despesasHTML}
                    <tr style="border-top: 2px solid #333; font-weight: bold;">
                        <td colspan="2">Total Despesas</td>
                        <td class="text-right">${formatCurrency(fechamento.totalDespesas)}</td>
                    </tr>
                </table>
                
                <h4>Taxas Bancárias</h4>
                <table style="width: 100%; border-collapse: collapse; margin-bottom: 20px;">
                    <tr style="border-bottom: 1px solid #ddd;">
                        <th style="text-align: left;">Total de Taxas</th>
                        <th style="text-align: right;">Valor</th>
                    </tr>
                    <tr>
                        <td>Total Taxas Bancárias</td>
                        <td class="text-right">${formatCurrency(fechamento.totalTaxas)}</td>
                    </tr>
                    <tr style="border-top: 2px solid #333; font-weight: bold;">
                        <td>Total Taxas</td>
                        <td class="text-right">${formatCurrency(fechamento.totalTaxas)}</td>
                    </tr>
                </table>
                
                <div style="border-top: 3px solid #333; padding: 15px; text-align: right; font-size: 1.2rem;">
                    <strong>Total Líquido do Dia: ${formatCurrency(fechamento.totalDiaria)}</strong>
                </div>
                
                <div class="relatorio-footer">
                    <p>Documento gerado em ${new Date().toLocaleString('pt-BR')}</p>
                    <p>${database.settings.systemName} - Sistema de Gerenciamento</p>
                </div>
            `;
            
            const relatorioConteudo = document.getElementById('relatorio-conteudo');
            if (relatorioConteudo) {
                relatorioConteudo.innerHTML = relatorioHTML;
            }
            
            const relatorioModal = document.getElementById('relatorio-modal');
            if (relatorioModal) {
                relatorioModal.style.display = 'flex';
            }
        }

        // ============== FECHAMENTO DIÁRIO (MANTIDO IGUAL) ==============
        function loadFechamentoDiarioContent() {
            const page = document.getElementById('fechamento-diario');
            if (!page) return;
            
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
                            <label class="fechamento-label">Centro de Custo</label>
                            <select class="fechamento-input" id="filtro-centro-fechamento">
                                <option value="">Todos</option>
                                ${getCentrosCustoOptions()}
                            </select>
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
                                <th>Centro</th>
                                <th>Dinheiro</th>
                                <th>PIX</th>
                                <th>Convênios</th>
                                <th>Despesas</th>
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
                                <button class="tab" data-tab="despesas">Despesas do Dia</button>
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
                                        <h4>Lançamento de Convênios</h4>
                                        ${currentUser.role !== 'Usuário' ? `
                                        <button type="button" class="btn btn-sm btn-primary" id="add-convenio-fechamento">
                                            <i class="fas fa-plus"></i> Novo Convênio
                                        </button>
                                        ` : ''}
                                    </div>
                                    <p class="text-muted">Adicione múltiplos convênios:</p>
                                    
                                    <div class="form-group">
                                        <label class="form-label">Selecione o Convênio</label>
                                        <select class="form-control" id="convenio-select">
                                            <option value="">Selecione um convênio...</option>
                                        </select>
                                    </div>
                                    
                                    <div class="fechamento-form">
                                        <div class="fechamento-group">
                                            <label class="fechamento-label">Valor do Convênio (R$)</label>
                                            <input type="number" step="0.01" class="fechamento-input" id="convenio-valor" placeholder="0,00">
                                        </div>
                                        <div class="fechamento-group">
                                            <label class="fechamento-label">&nbsp;</label>
                                            <button type="button" class="btn btn-primary" id="adicionar-convenio">
                                                <i class="fas fa-plus"></i> Adicionar Convênio
                                            </button>
                                        </div>
                                    </div>
                                </div>
                                
                                <div class="mt-3" id="convenios-selecionados-container">
                                    <h5>Convênios Lançados</h5>
                                    <div id="convenios-selecionados-lista" class="lancamentos-lista">
                                        <!-- Convênios lançados aparecerão aqui -->
                                    </div>
                                    <div class="mt-2 text-right">
                                        <strong>Total Convênios:</strong> 
                                        <span id="total-convenios-selecionados">R$ 0,00</span>
                                    </div>
                                </div>
                                
                                <div class="mt-3">
                                    <button type="button" class="btn btn-secondary" id="voltar-basico">
                                        <i class="fas fa-arrow-left"></i> Voltar
                                    </button>
                                    <button type="button" class="btn btn-primary" id="proximo-taxas">
                                        Próximo: Taxas Bancárias <i class="fas fa-arrow-right"></i>
                                    </button>
                                </div>
                            </div>
                            
                            <!-- Tab 3: Taxas Bancárias -->
                            <div class="tab-content" id="tab-taxas-bancarias">
                                <div class="mb-3">
                                    <div class="d-flex justify-between">
                                        <h4>Lançamento de Taxas Bancárias</h4>
                                        ${currentUser.role !== 'Usuário' ? `
                                        <button type="button" class="btn btn-sm btn-primary" id="add-banco-fechamento">
                                            <i class="fas fa-university"></i> Novo Banco
                                        </button>
                                        ` : ''}
                                    </div>
                                    <p class="text-muted">Adicione múltiplas taxas bancárias:</p>
                                    
                                    <div class="form-group">
                                        <label class="form-label">Selecione o Banco</label>
                                        <select class="form-control" id="taxa-banco-select">
                                            <option value="">Selecione um banco...</option>
                                        </select>
                                    </div>
                                    
                                    <div class="fechamento-form">
                                        <div class="fechamento-group">
                                            <label class="fechamento-label">Valor da Taxa (R$)</label>
                                            <input type="number" step="0.01" class="fechamento-input" id="taxa-valor" placeholder="0,00">
                                        </div>
                                        <div class="fechamento-group">
                                            <label class="fechamento-label">&nbsp;</label>
                                            <button type="button" class="btn btn-primary" id="adicionar-taxa">
                                                <i class="fas fa-plus"></i> Adicionar Taxa
                                            </button>
                                        </div>
                                    </div>
                                </div>
                                
                                <div class="mt-3" id="taxas-selecionadas-container">
                                    <h5>Taxas Bancárias Lançadas</h5>
                                    <div id="taxas-selecionadas-lista" class="lancamentos-lista">
                                        <!-- Taxas lançadas aparecerão aqui -->
                                    </div>
                                    <div class="mt-2 text-right">
                                        <strong>Total Taxas:</strong> 
                                        <span id="total-taxas-selecionadas">R$ 0,00</span>
                                    </div>
                                </div>
                                
                                <div class="mt-3">
                                    <button type="button" class="btn btn-secondary" id="voltar-convenios-taxas">
                                        <i class="fas fa-arrow-left"></i> Voltar para Convênios
                                    </button>
                                    <button type="button" class="btn btn-primary" id="proximo-despesas">
                                        Próximo: Despesas <i class="fas fa-arrow-right"></i>
                                    </button>
                                </div>
                            </div>
                            
                            <!-- Tab 4: Despesas do Dia -->
                            <div class="tab-content" id="tab-despesas">
                                <div class="mb-3">
                                    <div class="d-flex justify-between">
                                        <h4>Lançamento de Despesas do Dia</h4>
                                        <button type="button" class="btn btn-sm btn-primary" id="add-tipo-despesa">
                                            <i class="fas fa-plus"></i> Novo Tipo
                                        </button>
                                    </div>
                                    <p class="text-muted">Adicione as despesas do dia:</p>
                                    
                                    <div class="fechamento-form">
                                        <div class="fechamento-group">
                                            <label class="fechamento-label">Tipo de Despesa</label>
                                            <select class="fechamento-input" id="despesa-tipo-select">
                                                <option value="">Selecione...</option>
                                                ${database.tiposDespesa.map(t => `<option value="${t.id}">${t.nome}</option>`).join('')}
                                            </select>
                                        </div>
                                        <div class="fechamento-group">
                                            <label class="fechamento-label">Descrição</label>
                                            <input type="text" class="fechamento-input" id="despesa-descricao" placeholder="Ex: Posto Shell">
                                        </div>
                                        <div class="fechamento-group">
                                            <label class="fechamento-label">Valor (R$)</label>
                                            <input type="number" step="0.01" class="fechamento-input" id="despesa-valor" placeholder="0,00">
                                        </div>
                                        <div class="fechamento-group">
                                            <label class="fechamento-label">&nbsp;</label>
                                            <button type="button" class="btn btn-primary" id="adicionar-despesa">
                                                <i class="fas fa-plus"></i> Adicionar
                                            </button>
                                        </div>
                                    </div>
                                </div>
                                
                                <div class="mt-3" id="despesas-selecionadas-container">
                                    <h5>Despesas Lançadas</h5>
                                    <div id="despesas-selecionadas-lista" class="lancamentos-lista">
                                        <!-- Despesas lançadas aparecerão aqui -->
                                    </div>
                                    <div class="mt-2 text-right">
                                        <strong>Total Despesas:</strong> 
                                        <span id="total-despesas-selecionadas">R$ 0,00</span>
                                    </div>
                                </div>
                                
                                <div class="mt-3">
                                    <button type="button" class="btn btn-secondary" id="voltar-taxas-despesas">
                                        <i class="fas fa-arrow-left"></i> Voltar para Taxas
                                    </button>
                                    <button type="button" class="btn btn-primary" id="proximo-resumo">
                                        Próximo: Resumo <i class="fas fa-arrow-right"></i>
                                    </button>
                                </div>
                            </div>
                            
                            <!-- Tab 5: Resumo -->
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
                                        <span class="total-label">Despesas do Dia:</span>
                                        <span class="total-value" id="resumo-despesas">R$ 0,00</span>
                                    </div>
                                    <div class="total-item">
                                        <span class="total-label">Taxas Bancárias:</span>
                                        <span class="total-value text-danger" id="resumo-taxas">R$ 0,00</span>
                                    </div>
                                    <div class="total-item" style="border-top: 2px solid #333;">
                                        <span class="total-label"><strong>Total Líquido:</strong></span>
                                        <span class="total-value"><strong id="resumo-total-liquido">R$ 0,00</strong></span>
                                    </div>
                                </div>
                                
                                <div class="mt-3 d-flex justify-between">
                                    <button type="button" class="btn btn-secondary" id="voltar-despesas">
                                        <i class="fas fa-arrow-left"></i> Voltar
                                    </button>
                                    <div>
                                        <button type="button" class="btn btn-success" id="salvar-fechamento">
                                            <i class="fas fa-save"></i> Salvar Fechamento
                                        </button>
                                    </div>
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
        
        function setupFechamentoEvents() {
            if (!currentUser) return;
            
            // Novo fechamento
            const addFechamento = document.getElementById('add-fechamento');
            if (addFechamento) {
                addFechamento.addEventListener('click', function() {
                    if (currentUser.role === 'Usuário') {
                        showAlert('Usuários não podem criar novos fechamentos.', 'warning');
                        return;
                    }
                    
                    const formContainer = document.getElementById('fechamento-form-container');
                    if (formContainer) {
                        formContainer.style.display = 'block';
                    }
                    
                    resetFechamentoForm();
                    
                    const dataInput = document.getElementById('fechamento-data');
                    if (dataInput) dataInput.valueAsDate = new Date();
                    
                    const responsavelInput = document.getElementById('fechamento-responsavel');
                    if (responsavelInput) responsavelInput.value = currentUser.name;
                    
                    if (currentUser.role !== 'Administrador') {
                        const centroSelect = document.getElementById('fechamento-centro-custo');
                        if (centroSelect) centroSelect.value = currentUser.centroCusto;
                    }
                    
                    loadConveniosForSelection();
                    loadBancosForSelection();
                });
            }
            
            // Cancelar fechamento
            const cancelFechamento = document.getElementById('cancel-fechamento');
            if (cancelFechamento) {
                cancelFechamento.addEventListener('click', function() {
                    const formContainer = document.getElementById('fechamento-form-container');
                    if (formContainer) formContainer.style.display = 'none';
                    resetFechamentoForm();
                });
            }
            
            // Aplicar filtro
            const aplicarFiltro = document.getElementById('aplicar-filtro');
            if (aplicarFiltro) {
                aplicarFiltro.addEventListener('click', function() {
                    updateFechamentosTable();
                });
            }
            
            // Navegação entre tabs
            const proximoConvenios = document.getElementById('proximo-convenios');
            if (proximoConvenios) {
                proximoConvenios.addEventListener('click', function() {
                    showTab('convenios');
                });
            }
            
            const voltarBasico = document.getElementById('voltar-basico');
            if (voltarBasico) {
                voltarBasico.addEventListener('click', function() {
                    showTab('basico');
                });
            }
            
            const proximoTaxas = document.getElementById('proximo-taxas');
            if (proximoTaxas) {
                proximoTaxas.addEventListener('click', function() {
                    showTab('taxas-bancarias');
                });
            }
            
            const voltarConveniosTaxas = document.getElementById('voltar-convenios-taxas');
            if (voltarConveniosTaxas) {
                voltarConveniosTaxas.addEventListener('click', function() {
                    showTab('convenios');
                });
            }
            
            const proximoDespesas = document.getElementById('proximo-despesas');
            if (proximoDespesas) {
                proximoDespesas.addEventListener('click', function() {
                    showTab('despesas');
                });
            }
            
            const voltarTaxasDespesas = document.getElementById('voltar-taxas-despesas');
            if (voltarTaxasDespesas) {
                voltarTaxasDespesas.addEventListener('click', function() {
                    showTab('taxas-bancarias');
                });
            }
            
            const proximoResumo = document.getElementById('proximo-resumo');
            if (proximoResumo) {
                proximoResumo.addEventListener('click', function() {
                    showTab('resumo');
                });
            }
            
            const voltarDespesas = document.getElementById('voltar-despesas');
            if (voltarDespesas) {
                voltarDespesas.addEventListener('click', function() {
                    showTab('despesas');
                });
            }
            
            // Adicionar convênio
            const adicionarConvenio = document.getElementById('adicionar-convenio');
            if (adicionarConvenio) {
                adicionarConvenio.addEventListener('click', function() {
                    adicionarConvenioFechamento();
                });
            }
            
            // Adicionar taxa bancária
            const adicionarTaxa = document.getElementById('adicionar-taxa');
            if (adicionarTaxa) {
                adicionarTaxa.addEventListener('click', function() {
                    adicionarTaxaBancaria();
                });
            }
            
            // Adicionar despesa
            const adicionarDespesa = document.getElementById('adicionar-despesa');
            if (adicionarDespesa) {
                adicionarDespesa.addEventListener('click', function() {
                    adicionarDespesaFechamento();
                });
            }
            
            // Adicionar novo banco
            const addBancoFechamento = document.getElementById('add-banco-fechamento');
            if (addBancoFechamento) {
                addBancoFechamento.addEventListener('click', function() {
                    showBancoModal();
                });
            }

            // Adicionar novo tipo de despesa
            const addTipoDespesa = document.getElementById('add-tipo-despesa');
            if (addTipoDespesa) {
                addTipoDespesa.addEventListener('click', function() {
                    showTipoDespesaModal();
                });
            }

            // Salvar fechamento
            const salvarFechamento = document.getElementById('salvar-fechamento');
            if (salvarFechamento) {
                salvarFechamento.addEventListener('click', function() {
                    saveFechamento();
                });
            }
            
            // Adicionar novo convênio
            const addConvenioFechamento = document.getElementById('add-convenio-fechamento');
            if (addConvenioFechamento) {
                addConvenioFechamento.addEventListener('click', function() {
                    showConvenioModal();
                });
            }
            
            // Atualizar cálculo quando valores mudarem
            ['fechamento-dinheiro', 'fechamento-pix', 'fechamento-outros'].forEach(id => {
                const element = document.getElementById(id);
                if (element) {
                    element.addEventListener('input', updateResumoFechamento);
                }
            });
        }
        
        function adicionarConvenioFechamento() {
            const convenioSelect = document.getElementById('convenio-select');
            const valorInput = document.getElementById('convenio-valor');
            
            if (!convenioSelect || !valorInput) return;
            
            const convenioId = parseInt(convenioSelect.value);
            const valor = parseFloat(valorInput.value) || 0;
            
            if (!convenioId) {
                showAlert('Selecione um convênio.', 'warning');
                return;
            }
            
            if (valor <= 0) {
                showAlert('Informe um valor válido para o convênio.', 'warning');
                return;
            }
            
            const convenio = database.convenios.find(c => c.id === convenioId);
            if (!convenio) return;
            
            selectedConvenios.push({
                id: nextLancamentoId++,
                convenioId: convenioId,
                nome: convenio.nome,
                valor: valor
            });
            
            atualizarListaConvenios();
            
            valorInput.value = '';
            convenioSelect.focus();
            
            updateResumoFechamento();
        }
        
        function adicionarTaxaBancaria() {
            const bancoSelect = document.getElementById('taxa-banco-select');
            const valorInput = document.getElementById('taxa-valor');
            
            if (!bancoSelect || !valorInput) return;
            
            const bancoId = parseInt(bancoSelect.value);
            const valor = parseFloat(valorInput.value) || 0;
            
            if (!bancoId) {
                showAlert('Selecione um banco.', 'warning');
                return;
            }
            
            if (valor <= 0) {
                showAlert('Informe um valor válido para a taxa.', 'warning');
                return;
            }
            
            const banco = database.bancos.find(b => b.id === bancoId);
            if (!banco) return;
            
            selectedTaxasBancarias.push({
                id: nextLancamentoId++,
                bancoId: bancoId,
                bancoNome: banco.nome,
                valor: valor
            });
            
            atualizarListaTaxas();
            
            valorInput.value = '';
            bancoSelect.focus();
            
            updateResumoFechamento();
        }
        
        function adicionarDespesaFechamento() {
            const tipoSelect = document.getElementById('despesa-tipo-select');
            const descricaoInput = document.getElementById('despesa-descricao');
            const valorInput = document.getElementById('despesa-valor');
            
            if (!tipoSelect || !descricaoInput || !valorInput) return;
            
            const tipoId = parseInt(tipoSelect.value);
            const descricao = descricaoInput.value.trim();
            const valor = parseFloat(valorInput.value) || 0;
            
            if (!tipoId) {
                showAlert('Selecione o tipo de despesa.', 'warning');
                return;
            }
            
            if (!descricao) {
                showAlert('Informe uma descrição para a despesa.', 'warning');
                return;
            }
            
            if (valor <= 0) {
                showAlert('Informe um valor válido para a despesa.', 'warning');
                return;
            }
            
            const tipo = database.tiposDespesa.find(t => t.id === tipoId);
            if (!tipo) return;
            
            selectedDespesas.push({
                id: nextLancamentoId++,
                tipoId: tipoId,
                tipo: tipo.nome,
                descricao: descricao,
                valor: valor
            });
            
            atualizarListaDespesas();
            
            descricaoInput.value = '';
            valorInput.value = '';
            tipoSelect.focus();
            
            updateResumoFechamento();
        }
        
        function removerConvenio(id) {
            const index = selectedConvenios.findIndex(c => c.id === id);
            if (index !== -1) {
                selectedConvenios.splice(index, 1);
                atualizarListaConvenios();
                updateResumoFechamento();
            }
        }
        
        function removerTaxa(id) {
            const index = selectedTaxasBancarias.findIndex(t => t.id === id);
            if (index !== -1) {
                selectedTaxasBancarias.splice(index, 1);
                atualizarListaTaxas();
                updateResumoFechamento();
            }
        }
        
        function removerDespesa(id) {
            const index = selectedDespesas.findIndex(d => d.id === id);
            if (index !== -1) {
                selectedDespesas.splice(index, 1);
                atualizarListaDespesas();
                updateResumoFechamento();
            }
        }
        
        function atualizarListaConvenios() {
            const lista = document.getElementById('convenios-selecionados-lista');
            const totalSpan = document.getElementById('total-convenios-selecionados');
            
            if (!lista || !totalSpan) return;
            
            if (selectedConvenios.length === 0) {
                lista.innerHTML = '<div class="text-center p-3">Nenhum convênio lançado</div>';
                totalSpan.textContent = formatCurrency(0);
                return;
            }
            
            let html = '';
            let total = 0;
            
            selectedConvenios.forEach(convenio => {
                total += convenio.valor;
                html += `
                    <div class="lancamento-item">
                        <div class="lancamento-info">
                            <span class="lancamento-tipo">${convenio.nome}</span>
                            <div class="lancamento-descricao">Convênio</div>
                        </div>
                        <span class="lancamento-valor">${formatCurrency(convenio.valor)}</span>
                        <button class="btn-remove-lancamento" onclick="window.removerConvenio(${convenio.id})">
                            <i class="fas fa-times"></i>
                        </button>
                    </div>
                `;
            });
            
            lista.innerHTML = html;
            totalSpan.textContent = formatCurrency(total);
        }
        
        function atualizarListaTaxas() {
            const lista = document.getElementById('taxas-selecionadas-lista');
            const totalSpan = document.getElementById('total-taxas-selecionadas');
            
            if (!lista || !totalSpan) return;
            
            if (selectedTaxasBancarias.length === 0) {
                lista.innerHTML = '<div class="text-center p-3">Nenhuma taxa lançada</div>';
                totalSpan.textContent = formatCurrency(0);
                return;
            }
            
            let html = '';
            let total = 0;
            
            selectedTaxasBancarias.forEach(taxa => {
                total += taxa.valor;
                html += `
                    <div class="lancamento-item">
                        <div class="lancamento-info">
                            <span class="lancamento-tipo">${taxa.bancoNome}</span>
                            <div class="lancamento-descricao">Taxa bancária</div>
                        </div>
                        <span class="lancamento-valor">${formatCurrency(taxa.valor)}</span>
                        <button class="btn-remove-lancamento" onclick="window.removerTaxa(${taxa.id})">
                            <i class="fas fa-times"></i>
                        </button>
                    </div>
                `;
            });
            
            lista.innerHTML = html;
            totalSpan.textContent = formatCurrency(total);
        }
        
        function atualizarListaDespesas() {
            const lista = document.getElementById('despesas-selecionadas-lista');
            const totalSpan = document.getElementById('total-despesas-selecionadas');
            
            if (!lista || !totalSpan) return;
            
            if (selectedDespesas.length === 0) {
                lista.innerHTML = '<div class="text-center p-3">Nenhuma despesa lançada</div>';
                totalSpan.textContent = formatCurrency(0);
                return;
            }
            
            let html = '';
            let total = 0;
            
            selectedDespesas.forEach(despesa => {
                total += despesa.valor;
                html += `
                    <div class="lancamento-item">
                        <div class="lancamento-info">
                            <span class="lancamento-tipo">${despesa.tipo}</span>
                            <div class="lancamento-descricao">${despesa.descricao}</div>
                        </div>
                        <span class="lancamento-valor">${formatCurrency(despesa.valor)}</span>
                        <button class="btn-remove-lancamento" onclick="window.removerDespesa(${despesa.id})">
                            <i class="fas fa-times"></i>
                        </button>
                    </div>
                `;
            });
            
            lista.innerHTML = html;
            totalSpan.textContent = formatCurrency(total);
        }
        
        function showTab(tabName) {
            document.querySelectorAll('.tab-content').forEach(tab => {
                tab.classList.remove('active');
            });
            
            document.querySelectorAll('.tab').forEach(tab => {
                tab.classList.remove('active');
            });
            
            const tabContent = document.getElementById(`tab-${tabName}`);
            if (tabContent) tabContent.classList.add('active');
            
            const tabButton = document.querySelector(`.tab[data-tab="${tabName}"]`);
            if (tabButton) tabButton.classList.add('active');
        }
        
        function resetFechamentoForm() {
            const form = document.getElementById('fechamento-form-basico');
            if (form) form.reset();
            
            selectedConvenios = [];
            selectedTaxasBancarias = [];
            selectedDespesas = [];
            nextLancamentoId = 1;
            
            atualizarListaConvenios();
            atualizarListaTaxas();
            atualizarListaDespesas();
            
            showTab('basico');
        }
        
        function loadConveniosForSelection() {
            const select = document.getElementById('convenio-select');
            if (!select) return;
            
            let conveniosAtivos = database.convenios.filter(c => c.status === 'Ativo');
            conveniosAtivos = filterByUserCentro(conveniosAtivos);
            
            let options = '<option value="">Selecione um convênio...</option>';
            conveniosAtivos.forEach(convenio => {
                options += `<option value="${convenio.id}">${convenio.nome} - ${convenio.documento}</option>`;
            });
            
            select.innerHTML = options;
        }
        
        function loadBancosForSelection() {
            const select = document.getElementById('taxa-banco-select');
            if (!select) return;
            
            let bancosAtivos = database.bancos.filter(b => b.status === 'Ativo');
            
            let options = '<option value="">Selecione um banco...</option>';
            bancosAtivos.forEach(banco => {
                options += `<option value="${banco.id}">${banco.nome} - Ag: ${banco.agencia}</option>`;
            });
            
            select.innerHTML = options;
        }
        
        function updateResumoFechamento() {
            const dinheiro = parseFloat(document.getElementById('fechamento-dinheiro')?.value) || 0;
            const pix = parseFloat(document.getElementById('fechamento-pix')?.value) || 0;
            const outros = parseFloat(document.getElementById('fechamento-outros')?.value) || 0;
            
            const totalConvenios = selectedConvenios.reduce((sum, c) => sum + c.valor, 0);
            const totalTaxas = selectedTaxasBancarias.reduce((sum, t) => sum + t.valor, 0);
            const totalDespesas = selectedDespesas.reduce((sum, d) => sum + d.valor, 0);
            
            const totalFaturamentos = dinheiro + pix + outros + totalConvenios;
            const totalLiquido = totalFaturamentos - totalDespesas - totalTaxas;
            
            const resumoDinheiro = document.getElementById('resumo-dinheiro');
            const resumoPix = document.getElementById('resumo-pix');
            const resumoOutros = document.getElementById('resumo-outros');
            const resumoConvenios = document.getElementById('resumo-convenios');
            const resumoTotalFaturamentos = document.getElementById('resumo-total-faturamentos');
            const resumoDespesas = document.getElementById('resumo-despesas');
            const resumoTaxas = document.getElementById('resumo-taxas');
            const resumoTotalLiquido = document.getElementById('resumo-total-liquido');
            
            if (resumoDinheiro) resumoDinheiro.textContent = formatCurrency(dinheiro);
            if (resumoPix) resumoPix.textContent = formatCurrency(pix);
            if (resumoOutros) resumoOutros.textContent = formatCurrency(outros);
            if (resumoConvenios) resumoConvenios.textContent = formatCurrency(totalConvenios);
            if (resumoTotalFaturamentos) resumoTotalFaturamentos.textContent = formatCurrency(totalFaturamentos);
            if (resumoDespesas) resumoDespesas.textContent = formatCurrency(totalDespesas);
            if (resumoTaxas) resumoTaxas.textContent = formatCurrency(totalTaxas);
            if (resumoTotalLiquido) resumoTotalLiquido.textContent = formatCurrency(totalLiquido);
        }
        
        function saveFechamento() {
            if (currentUser.role === 'Usuário') {
                showAlert('Usuários não podem salvar fechamentos.', 'warning');
                return;
            }
            
            const data = document.getElementById('fechamento-data')?.value;
            const responsavel = document.getElementById('fechamento-responsavel')?.value;
            let centroCusto = document.getElementById('fechamento-centro-custo')?.value;
            
            if (currentUser.role !== 'Administrador') {
                centroCusto = currentUser.centroCusto;
            }
            
            if (!data || !responsavel || !centroCusto) {
                showAlert('Preencha todos os campos obrigatórios.', 'warning');
                showTab('basico');
                return;
            }
            
            const dinheiro = parseFloat(document.getElementById('fechamento-dinheiro')?.value) || 0;
            const pix = parseFloat(document.getElementById('fechamento-pix')?.value) || 0;
            const outros = parseFloat(document.getElementById('fechamento-outros')?.value) || 0;
            
            const totalConvenios = selectedConvenios.reduce((sum, c) => sum + c.valor, 0);
            const totalTaxas = selectedTaxasBancarias.reduce((sum, t) => sum + t.valor, 0);
            const totalDespesas = selectedDespesas.reduce((sum, d) => sum + d.valor, 0);
            
            const totalFaturamentos = dinheiro + pix + outros + totalConvenios;
            const totalLiquido = totalFaturamentos - totalDespesas - totalTaxas;
            
            const novoFechamento = {
                id: database.fechamentosDiarios.length + 1,
                data: data,
                responsavel: responsavel,
                centroCusto: centroCusto,
                dinheiro: dinheiro,
                pix: pix,
                outrosFaturamentos: outros,
                convenios: selectedConvenios.map(c => ({
                    id: c.id,
                    convenioId: c.convenioId,
                    nome: c.nome,
                    valor: c.valor
                })),
                taxasBancarias: selectedTaxasBancarias.map(t => ({
                    id: t.id,
                    bancoId: t.bancoId,
                    bancoNome: t.bancoNome,
                    valor: t.valor
                })),
                despesas: selectedDespesas.map(d => ({
                    id: d.id,
                    tipo: d.tipo,
                    descricao: d.descricao,
                    valor: d.valor
                })),
                totalFaturamentos: totalFaturamentos,
                totalDespesas: totalDespesas,
                totalTaxas: totalTaxas,
                totalDiaria: totalLiquido,
                status: "Fechado",
                createdAt: new Date().toISOString(),
                createdBy: currentUser.id
            };
            
            database.fechamentosDiarios.push(novoFechamento);
            
            const formContainer = document.getElementById('fechamento-form-container');
            if (formContainer) formContainer.style.display = 'none';
            
            updateFechamentosTable();
            
            if (currentPage === 'dashboard') {
                atualizarDashboardComFiltros();
            }
            
            showAlert('Fechamento salvo com sucesso!', 'success');
            
            if (confirm('Deseja imprimir o relatório deste fechamento?')) {
                imprimirRelatorioFechamento(novoFechamento);
            }
            
            resetFechamentoForm();
        }
        
        function updateFechamentosTable() {
            const tbody = document.getElementById('fechamentos-table');
            if (!tbody) return;
            
            const filtroMes = document.getElementById('filtro-mes')?.value;
            const filtroCentro = document.getElementById('filtro-centro-fechamento')?.value;
            
            let fechamentos = database.fechamentosDiarios;
            
            fechamentos = filterByUserCentro(fechamentos);
            
            if (filtroMes) {
                const [ano, mes] = filtroMes.split('-');
                fechamentos = fechamentos.filter(f => {
                    const data = new Date(f.data);
                    return data.getFullYear() == ano && (data.getMonth() + 1) == mes;
                });
            }
            
            if (filtroCentro) {
                fechamentos = fechamentos.filter(f => f.centroCusto === filtroCentro);
            }
            
            fechamentos.sort((a, b) => new Date(b.data) - new Date(a.data));
            
            if (fechamentos.length === 0) {
                tbody.innerHTML = '<tr><td colspan="9" class="text-center">Nenhum fechamento encontrado</td></tr>';
                return;
            }
            
            let html = '';
            fechamentos.forEach(f => {
                const totalConvenios = f.convenios ? f.convenios.reduce((sum, c) => sum + c.valor, 0) : 0;
                const totalDespesas = f.despesas ? f.despesas.reduce((sum, d) => sum + d.valor, 0) : 0;
                
                html += `
                    <tr>
                        <td>${formatDate(f.data)}</td>
                        <td>${f.responsavel}</td>
                        <td>${f.centroCusto}</td>
                        <td>${formatCurrency(f.dinheiro)}</td>
                        <td>${formatCurrency(f.pix)}</td>
                        <td>${formatCurrency(totalConvenios)}</td>
                        <td>${formatCurrency(totalDespesas)}</td>
                        <td><strong>${formatCurrency(f.totalDiaria)}</strong></td>
                        <td>
                            <button class="btn-icon" onclick="window.visualizarFechamento(${f.id})" title="Visualizar">
                                <i class="fas fa-eye"></i>
                            </button>
                            <button class="btn-icon" onclick="window.imprimirFechamento(${f.id})" title="Imprimir">
                                <i class="fas fa-print"></i>
                            </button>
                        </td>
                    </tr>
                `;
            });
            
            tbody.innerHTML = html;
        }
        
        function visualizarFechamento(id) {
            const fechamento = database.fechamentosDiarios.find(f => f.id === id);
            if (fechamento) {
                imprimirRelatorioFechamento(fechamento);
            }
        }
        
        function imprimirFechamento(id) {
            const fechamento = database.fechamentosDiarios.find(f => f.id === id);
            if (fechamento) {
                imprimirRelatorioFechamento(fechamento);
            }
        }
        
        function showBancoModal() {
            if (document.getElementById('banco-modal')) {
                document.getElementById('banco-modal').remove();
            }
            
            const modalHTML = `
                <div id="banco-modal" class="modal" style="display: flex;">
                    <div class="modal-content">
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
            
            document.body.insertAdjacentHTML('beforeend', modalHTML);
            
            const cancelBanco = document.getElementById('cancel-banco');
            if (cancelBanco) {
                cancelBanco.addEventListener('click', function() {
                    const modal = document.getElementById('banco-modal');
                    if (modal) modal.remove();
                });
            }
            
            const bancoForm = document.getElementById('banco-form');
            if (bancoForm) {
                bancoForm.addEventListener('submit', function(e) {
                    e.preventDefault();
                    saveBanco();
                });
            }
        }
        
        function saveBanco() {
            const nome = document.getElementById('banco-nome')?.value;
            const agencia = document.getElementById('banco-agencia')?.value;
            const conta = document.getElementById('banco-conta')?.value;
            const status = document.getElementById('banco-status')?.value;
            
            if (!nome || !agencia || !conta || !status) return;
            
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
            
            const modal = document.getElementById('banco-modal');
            if (modal) modal.remove();
            
            if (currentPage === 'fechamento-diario') {
                loadBancosForSelection();
            }
            
            showAlert('Banco cadastrado com sucesso!', 'success');
        }
        
        function showTipoDespesaModal() {
            const nome = prompt('Digite o nome do novo tipo de despesa:');
            if (nome && nome.trim()) {
                const novoTipo = {
                    id: database.tiposDespesa.length + 1,
                    nome: nome.trim(),
                    icone: 'fa-tag'
                };
                
                database.tiposDespesa.push(novoTipo);
                
                const select = document.getElementById('despesa-tipo-select');
                if (select) {
                    const option = document.createElement('option');
                    option.value = novoTipo.id;
                    option.textContent = novoTipo.nome;
                    select.appendChild(option);
                }
                
                showAlert('Tipo de despesa cadastrado com sucesso!', 'success');
            }
        }

        // ============== SISTEMA DE CONVÊNIOS ==============
        function loadConveniosContent() {
            const page = document.getElementById('convenios');
            if (!page) return;
            
            if (currentUser.role === 'Usuário') {
                page.innerHTML = `
                    <div class="restricted-access">
                        <i class="fas fa-ban"></i>
                        <h3>Acesso Restrito</h3>
                        <p>Usuários não têm acesso à funcionalidade de convênios.</p>
                    </div>
                `;
                return;
            }
            
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
            const addConvenio = document.getElementById('add-convenio');
            if (addConvenio) {
                addConvenio.addEventListener('click', function() {
                    showConvenioModal();
                });
            }
            
            const aplicarFiltro = document.getElementById('aplicar-filtro-convenios');
            if (aplicarFiltro) {
                aplicarFiltro.addEventListener('click', function() {
                    updateConveniosTable();
                });
            }
        }
        
        function showConvenioModal() {
            const convenioForm = document.getElementById('convenio-form');
            if (convenioForm) convenioForm.reset();
            
            const select = document.getElementById('convenio-centro-custo');
            if (!select) return;
            
            select.innerHTML = '<option value="">Nenhum</option>';
            
            let centros = database.centrosCusto;
            if (currentUser.role !== 'Administrador') {
                centros = centros.filter(c => c.codigo === currentUser.centroCusto);
            }
            
            centros.forEach(centro => {
                const option = document.createElement('option');
                option.value = centro.codigo;
                option.textContent = `${centro.codigo} - ${centro.nome}`;
                select.appendChild(option);
            });
            
            const convenioModal = document.getElementById('convenio-modal');
            if (convenioModal) {
                convenioModal.style.display = 'flex';
            }
        }
        
        function saveConvenio() {
            const nome = document.getElementById('convenio-nome')?.value;
            const documento = document.getElementById('convenio-documento')?.value;
            const tipo = document.getElementById('convenio-tipo')?.value;
            const centroCusto = document.getElementById('convenio-centro-custo')?.value;
            const status = document.getElementById('convenio-status')?.value;
            
            if (!nome || !documento || !tipo || !status) {
                showAlert('Preencha todos os campos obrigatórios.', 'warning');
                return;
            }
            
            if (!validarDocumento(documento, tipo)) {
                showAlert('Documento inválido para o tipo selecionado.', 'danger');
                return;
            }
            
            const novoConvenio = {
                id: database.convenios.length + 1,
                nome: nome,
                documento: documento,
                tipo: tipo,
                centroCusto: centroCusto || currentUser.centroCusto,
                status: status,
                createdAt: new Date().toISOString(),
                createdBy: currentUser.id
            };
            
            database.convenios.push(novoConvenio);
            
            const convenioModal = document.getElementById('convenio-modal');
            if (convenioModal) convenioModal.style.display = 'none';
            
            updateConveniosTable();
            
            if (currentPage === 'fechamento-diario') {
                loadConveniosForSelection();
            }
            
            showAlert('Convênio cadastrado com sucesso!', 'success');
        }
        
        function validarDocumento(documento, tipo) {
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
            
            const filtroStatus = document.getElementById('filtro-status-convenio')?.value;
            const filtroCentro = document.getElementById('filtro-centro-convenio')?.value;
            
            let convenios = database.convenios;
            
            convenios = filterByUserCentro(convenios);
            
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
                            <button class="btn-icon" onclick="window.editConvenio(${c.id})" title="Editar">
                                <i class="fas fa-edit"></i>
                            </button>
                            <button class="btn-icon" onclick="window.toggleConvenioStatus(${c.id})" title="${c.status === 'Ativo' ? 'Desativar' : 'Ativar'}">
                                <i class="fas ${c.status === 'Ativo' ? 'fa-ban' : 'fa-check'}"></i>
                            </button>
                        </td>
                    </tr>
                `;
            });
            
            tbody.innerHTML = html;
        }
        
        function editConvenio(id) {
            const convenio = database.convenios.find(c => c.id === id);
            if (!convenio) return;
            
            const nomeInput = document.getElementById('convenio-nome');
            const documentoInput = document.getElementById('convenio-documento');
            const tipoSelect = document.getElementById('convenio-tipo');
            const statusSelect = document.getElementById('convenio-status');
            
            if (nomeInput) nomeInput.value = convenio.nome;
            if (documentoInput) documentoInput.value = convenio.documento;
            if (tipoSelect) tipoSelect.value = convenio.tipo;
            if (statusSelect) statusSelect.value = convenio.status;
            
            const select = document.getElementById('convenio-centro-custo');
            if (select) {
                select.innerHTML = '<option value="">Nenhum</option>';
                
                let centros = database.centrosCusto;
                if (currentUser.role !== 'Administrador') {
                    centros = centros.filter(c => c.codigo === currentUser.centroCusto);
                }
                
                centros.forEach(centro => {
                    const option = document.createElement('option');
                    option.value = centro.codigo;
                    option.textContent = `${centro.codigo} - ${centro.nome}`;
                    if (centro.codigo === convenio.centroCusto) {
                        option.selected = true;
                    }
                    select.appendChild(option);
                });
            }
            
            const convenioModal = document.getElementById('convenio-modal');
            if (convenioModal) {
                convenioModal.style.display = 'flex';
            }
            
            const form = document.getElementById('convenio-form');
            if (!form) return;
            
            const originalSubmit = form.onsubmit;
            
            form.onsubmit = function(e) {
                e.preventDefault();
                
                convenio.nome = document.getElementById('convenio-nome')?.value || convenio.nome;
                convenio.documento = document.getElementById('convenio-documento')?.value || convenio.documento;
                convenio.tipo = document.getElementById('convenio-tipo')?.value || convenio.tipo;
                convenio.centroCusto = document.getElementById('convenio-centro-custo')?.value || convenio.centroCusto;
                convenio.status = document.getElementById('convenio-status')?.value || convenio.status;
                
                if (convenioModal) convenioModal.style.display = 'none';
                
                updateConveniosTable();
                
                showAlert('Convênio atualizado com sucesso!', 'success');
                
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

        // ============== SISTEMA DE USUÁRIOS ==============
        function loadUsuariosContent() {
            const page = document.getElementById('usuarios');
            if (!page) return;
            
            if (currentUser.role !== "Administrador") {
                page.innerHTML = `
                    <div class="restricted-access">
                        <i class="fas fa-ban"></i>
                        <h3>Acesso Restrito</h3>
                        <p>Você não tem permissão para gerenciar usuários.</p>
                        <p>Apenas administradores podem acessar esta funcionalidade.</p>
                    </div>
                `;
                return;
            }
            
            page.innerHTML = `
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
                                <th>Centro de Custo</th>
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
            `;
            
            setupUsuariosEvents();
            updateUsuariosTable();
        }
        
        function setupUsuariosEvents() {
            const addUsuarioBtn = document.getElementById('add-usuario-btn');
            if (addUsuarioBtn) {
                addUsuarioBtn.addEventListener('click', function() {
                    showUsuarioModal();
                });
            }
        }
        
        function showUsuarioModal(usuario = null) {
            const modal = document.getElementById('usuario-modal');
            const title = document.getElementById('usuario-modal-title');
            const form = document.getElementById('usuario-form');
            
            if (!modal || !title || !form) return;
            
            if (usuario) {
                title.textContent = 'Editar Usuário';
                const usuarioId = document.getElementById('usuario-id');
                const usuarioNome = document.getElementById('usuario-nome');
                const usuarioEmail = document.getElementById('usuario-email');
                const usuarioPerfil = document.getElementById('usuario-perfil');
                const usuarioStatus = document.getElementById('usuario-status');
                
                if (usuarioId) usuarioId.value = usuario.id;
                if (usuarioNome) usuarioNome.value = usuario.name;
                if (usuarioEmail) usuarioEmail.value = usuario.email;
                if (usuarioPerfil) usuarioPerfil.value = usuario.role;
                if (usuarioStatus) usuarioStatus.value = usuario.status;
                
                const centroSelect = document.getElementById('usuario-centro-custo');
                if (centroSelect) centroSelect.value = usuario.centroCusto || '';
            } else {
                title.textContent = 'Novo Usuário';
                form.reset();
                const usuarioId = document.getElementById('usuario-id');
                if (usuarioId) usuarioId.value = '';
            }
            
            const centroSelect = document.getElementById('usuario-centro-custo');
            if (centroSelect) {
                centroSelect.innerHTML = '<option value="">Selecione...</option>';
                database.centrosCusto.forEach(centro => {
                    const option = document.createElement('option');
                    option.value = centro.codigo;
                    option.textContent = `${centro.codigo} - ${centro.nome}`;
                    centroSelect.appendChild(option);
                });
            }
            
            modal.style.display = 'flex';
        }
        
        function saveUsuario() {
            const id = document.getElementById('usuario-id')?.value;
            const nome = document.getElementById('usuario-nome')?.value;
            const email = document.getElementById('usuario-email')?.value;
            const senha = document.getElementById('usuario-senha')?.value;
            const perfil = document.getElementById('usuario-perfil')?.value;
            const centroCusto = document.getElementById('usuario-centro-custo')?.value;
            const status = document.getElementById('usuario-status')?.value;
            
            if (!nome || !email || !perfil || !centroCusto) {
                showAlert('Preencha todos os campos obrigatórios.', 'warning');
                return;
            }
            
            if (id) {
                const usuario = database.users.find(u => u.id === parseInt(id));
                if (usuario) {
                    usuario.name = nome;
                    usuario.email = email;
                    if (senha) usuario.password = senha;
                    usuario.role = perfil;
                    usuario.centroCusto = centroCusto;
                    usuario.status = status;
                    
                    showAlert('Usuário atualizado com sucesso!', 'success');
                }
            } else {
                if (!senha) {
                    showAlert('Informe uma senha para o novo usuário.', 'warning');
                    return;
                }
                
                const novoUsuario = {
                    id: database.users.length + 1,
                    name: nome,
                    email: email,
                    password: senha,
                    role: perfil,
                    centroCusto: centroCusto,
                    lastLogin: new Date().toISOString(),
                    status: status
                };
                
                database.users.push(novoUsuario);
                showAlert('Usuário cadastrado com sucesso!', 'success');
            }
            
            const usuarioModal = document.getElementById('usuario-modal');
            if (usuarioModal) usuarioModal.style.display = 'none';
            
            updateUsuariosTable();
        }
        
        function updateUsuariosTable() {
            const tbody = document.getElementById('usuarios-table');
            if (!tbody) return;
            
            let html = '';
            database.users.forEach(user => {
                const centro = database.centrosCusto.find(c => c.codigo === user.centroCusto);
                const centroNome = centro ? `${centro.codigo} - ${centro.nome}` : 'Não vinculado';
                
                html += `
                    <tr>
                        <td>${user.id}</td>
                        <td>${user.name}</td>
                        <td>${user.email}</td>
                        <td><span class="badge ${user.role === 'Administrador' ? 'badge-danger' : user.role === 'Gerente' ? 'badge-warning' : 'badge-info'}">${user.role}</span></td>
                        <td>${centroNome}</td>
                        <td>${formatDate(user.lastLogin)}</td>
                        <td><span class="badge ${user.status === 'Ativo' ? 'badge-success' : 'badge-danger'}">${user.status}</span></td>
                        <td>
                            <button class="btn-icon" onclick="window.editUsuario(${user.id})" title="Editar">
                                <i class="fas fa-edit"></i>
                            </button>
                            ${user.id !== currentUser.id ? `
                            <button class="btn-icon" onclick="window.toggleUsuarioStatus(${user.id})" 
                                    title="${user.status === 'Ativo' ? 'Desativar' : 'Ativar'}">
                                <i class="fas ${user.status === 'Ativo' ? 'fa-user-slash' : 'fa-user-check'}"></i>
                            </button>
                            ` : ''}
                        </td>
                    </tr>
                `;
            });
            
            tbody.innerHTML = html;
        }
        
        function editUsuario(id) {
            const usuario = database.users.find(u => u.id === id);
            if (usuario) {
                showUsuarioModal(usuario);
            }
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

        // ============== DEMAIS FUNÇÕES ==============
        function loadLancamentosContent() {
            const page = document.getElementById('lancamentos');
            if (!page) return;
            
            page.innerHTML = `
                <div class="d-flex justify-between mb-3">
                    <h2>Lançamentos</h2>
                    <button class="btn btn-primary" id="add-lancamento-btn">
                        <i class="fas fa-plus"></i> Novo Lançamento
                    </button>
                </div>
                
                <!-- Tabela de Lançamentos -->
                <div class="table-container">
                    <table class="table">
                        <thead>
                            <tr>
                                <th>Data</th>
                                <th>Descrição</th>
                                <th>Centro de Custo</th>
                                <th>Valor</th>
                                <th>Categoria</th>
                                <th>Ações</th>
                            </tr>
                        </thead>
                        <tbody id="lancamentos-table">
                            <!-- Dados serão carregados via JavaScript -->
                        </tbody>
                    </table>
                </div>
            `;
            
            updateLancamentosTable();
        }
        
        function updateLancamentosTable() {
            const tbody = document.getElementById('lancamentos-table');
            if (!tbody) return;
            
            let lancamentos = filterByUserCentro(database.lancamentos);
            lancamentos.sort((a, b) => new Date(b.data) - new Date(a.data));
            
            if (lancamentos.length === 0) {
                tbody.innerHTML = '<tr><td colspan="6" class="text-center">Nenhum lançamento encontrado</td></tr>';
                return;
            }
            
            let html = '';
            lancamentos.forEach(l => {
                html += `
                    <tr>
                        <td>${formatDate(l.data)}</td>
                        <td>${l.descricao}</td>
                        <td>${l.centroCusto}</td>
                        <td>${formatCurrency(l.valor)}</td>
                        <td><span class="badge ${getCategoriaClass(l.categoria)}">${l.categoria}</span></td>
                        <td>
                            <button class="btn-icon" onclick="window.visualizarLancamento(${l.id})" title="Visualizar">
                                <i class="fas fa-eye"></i>
                            </button>
                        </td>
                    </tr>
                `;
            });
            
            tbody.innerHTML = html;
        }
        
        function visualizarLancamento(id) {
            const lancamento = database.lancamentos.find(l => l.id === id);
            if (lancamento) {
                showAlert(`Lançamento: ${lancamento.descricao} - ${formatCurrency(lancamento.valor)}`, 'info');
            }
        }
        
        function loadCentroCustoContent() {
            const page = document.getElementById('centro-custo');
            if (!page) return;
            
            if (currentUser.role === 'Usuário') {
                page.innerHTML = `
                    <div class="restricted-access">
                        <i class="fas fa-ban"></i>
                        <h3>Acesso Restrito</h3>
                        <p>Usuários não têm acesso à funcionalidade de centros de custo.</p>
                    </div>
                `;
                return;
            }
            
            page.innerHTML = `
                <h2>Centros de Custo</h2>
                
                <div class="table-container">
                    <table class="table">
                        <thead>
                            <tr>
                                <th>Código</th>
                                <th>Nome</th>
                                <th>Responsável</th>
                                <th>Orçamento</th>
                                <th>Gasto</th>
                                <th>Saldo</th>
                                <th>Status</th>
                            </tr>
                        </thead>
                        <tbody id="centro-custo-table">
                            <!-- Dados serão carregados via JavaScript -->
                        </tbody>
                    </table>
                </div>
            `;
            
            updateCentroCustoTable();
        }
        
        function updateCentroCustoTable() {
            const tbody = document.getElementById('centro-custo-table');
            if (!tbody) return;
            
            let centros = getCentrosCustoForUser();
            
            if (centros.length === 0) {
                tbody.innerHTML = '<tr><td colspan="7" class="text-center">Nenhum centro de custo encontrado</td></tr>';
                return;
            }
            
            let html = '';
            centros.forEach(centro => {
                const saldo = centro.orcamento - centro.gasto;
                
                html += `
                    <tr>
                        <td><strong>${centro.codigo}</strong></td>
                        <td>${centro.nome}</td>
                        <td>${centro.responsavel}</td>
                        <td>${formatCurrency(centro.orcamento)}</td>
                        <td>${formatCurrency(centro.gasto)}</td>
                        <td class="${saldo < 0 ? 'danger-text' : 'success-text'}">${formatCurrency(saldo)}</td>
                        <td><span class="badge ${centro.status === 'Ativo' ? 'badge-success' : 'badge-danger'}">${centro.status}</span></td>
                    </tr>
                `;
            });
            
            tbody.innerHTML = html;
        }
        
        function loadUploadExcelContent() {
            const page = document.getElementById('upload-excel');
            if (!page) return;
            
            if (currentUser.role === 'Usuário') {
                page.innerHTML = `
                    <div class="restricted-access">
                        <i class="fas fa-ban"></i>
                        <h3>Acesso Restrito</h3>
                        <p>Usuários não têm acesso à importação de arquivos.</p>
                    </div>
                `;
                return;
            }
            
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
                        <button class="btn btn-success btn-block" id="import-btn">
                            <i class="fas fa-upload"></i> Importar Dados
                        </button>
                    </div>
                </div>
            `;
            
            setupFileUpload();
        }
        
        function setupFileUpload() {
            const dropArea = document.getElementById('drop-area');
            const fileInput = document.getElementById('file-input');
            const browseBtn = document.getElementById('browse-btn');
            
            if (!dropArea || !fileInput || !browseBtn) return;
            
            ['dragenter', 'dragover', 'dragleave', 'drop'].forEach(eventName => {
                dropArea.addEventListener(eventName, preventDefaults, false);
            });
            
            function preventDefaults(e) {
                e.preventDefault();
                e.stopPropagation();
            }
            
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
            
            dropArea.addEventListener('drop', handleDrop, false);
            
            function handleDrop(e) {
                const dt = e.dataTransfer;
                const files = dt.files;
                
                if (files.length) {
                    handleFiles(files);
                }
            }
            
            browseBtn.addEventListener('click', function() {
                fileInput.click();
            });
            
            fileInput.addEventListener('change', function() {
                if (this.files.length) {
                    handleFiles(this.files);
                }
            });
            
            const importBtn = document.getElementById('import-btn');
            if (importBtn) {
                importBtn.addEventListener('click', simulateExcelImport);
            }
        }
        
        function handleFiles(files) {
            const file = files[0];
            
            if (!file.name.match(/\.(xlsx|xls)$/i)) {
                alert('Por favor, selecione um arquivo Excel (.xlsx ou .xls)');
                return;
            }
            
            const selectedFileName = document.getElementById('selected-file-name');
            if (selectedFileName) {
                selectedFileName.textContent = file.name + ' (' + (file.size / 1024).toFixed(2) + ' KB)';
            }
            
            const fileInfo = document.getElementById('file-info');
            if (fileInfo) {
                fileInfo.style.display = 'block';
            }
        }
        
        function simulateExcelImport() {
            showAlert('Importação simulada: 15 registros importados com sucesso!', 'success');
            const fileInfo = document.getElementById('file-info');
            if (fileInfo) fileInfo.style.display = 'none';
        }
        
        function loadRelatoriosContent() {
            const page = document.getElementById('relatorios');
            if (!page) return;
            
            if (currentUser.role === 'Usuário') {
                page.innerHTML = `
                    <div class="restricted-access">
                        <i class="fas fa-ban"></i>
                        <h3>Acesso Restrito</h3>
                        <p>Usuários não têm acesso à funcionalidade de relatórios.</p>
                    </div>
                `;
                return;
            }
            
            page.innerHTML = `
                <h2>Relatórios</h2>
                
                <div class="dashboard-customizer mb-3">
                    <h3>Gerar Relatórios</h3>
                    
                    <div class="fechamento-form">
                        <div class="fechamento-group">
                            <label class="fechamento-label">Tipo de Relatório</label>
                            <select class="fechamento-input" id="report-type">
                                <option value="fechamentos">Fechamentos Diários</option>
                                <option value="convenios">Convênios</option>
                                <option value="lancamentos">Lançamentos</option>
                                <option value="centros">Centros de Custo</option>
                                <option value="taxas">Taxas Bancárias</option>
                                <option value="despesas">Despesas</option>
                            </select>
                        </div>
                        
                        <div class="fechamento-group">
                            <label class="fechamento-label">Período</label>
                            <input type="month" class="fechamento-input" id="report-month" 
                                   value="${new Date().toISOString().substring(0, 7)}">
                        </div>
                        
                        <div class="fechamento-group">
                            <label class="fechamento-label">Centro de Custo</label>
                            <select class="fechamento-input" id="report-centro">
                                <option value="">Todos</option>
                                ${getCentrosCustoOptions()}
                            </select>
                        </div>
                        
                        <div class="fechamento-group">
                            <label class="fechamento-label">&nbsp;</label>
                            <button class="btn btn-primary" id="generate-report-btn">
                                <i class="fas fa-chart-bar"></i> Gerar Relatório
                            </button>
                        </div>
                    </div>
                </div>
                
                <div id="report-result" class="table-container">
                    <!-- Resultado do relatório será carregado aqui -->
                </div>
            `;
            
            setupReportEvents();
        }
        
        function setupReportEvents() {
            const generateReportBtn = document.getElementById('generate-report-btn');
            if (generateReportBtn) {
                generateReportBtn.addEventListener('click', function() {
                    generateReport();
                });
            }
        }
        
        function generateReport() {
            const tipo = document.getElementById('report-type')?.value;
            const mes = document.getElementById('report-month')?.value;
            const centro = document.getElementById('report-centro')?.value;
            
            if (!tipo) return;
            
            let dados = [];
            let titulo = '';
            let headers = [];
            
            switch(tipo) {
                case 'fechamentos':
                    titulo = 'Relatório de Fechamentos Diários';
                    headers = ['Data', 'Responsável', 'Centro', 'Faturamento', 'Despesas', 'Taxas', 'Total Líquido'];
                    
                    dados = filterByUserCentro(database.fechamentosDiarios);
                    if (centro) dados = dados.filter(f => f.centroCusto === centro);
                    if (mes) {
                        const [ano, mesNum] = mes.split('-');
                        dados = dados.filter(f => {
                            const data = new Date(f.data);
                            return data.getFullYear() == ano && (data.getMonth() + 1) == mesNum;
                        });
                    }
                    break;
                    
                case 'convenios':
                    titulo = 'Relatório de Convênios';
                    headers = ['Nome', 'Documento', 'Tipo', 'Centro', 'Status'];
                    
                    dados = filterByUserCentro(database.convenios);
                    if (centro) dados = dados.filter(c => c.centroCusto === centro);
                    break;
                    
                case 'taxas':
                    titulo = 'Relatório de Taxas Bancárias';
                    headers = ['Data', 'Banco', 'Valor', 'Centro'];
                    
                    dados = [];
                    const fechamentos = filterByUserCentro(database.fechamentosDiarios);
                    fechamentos.forEach(f => {
                        if (f.taxasBancarias) {
                            f.taxasBancarias.forEach(t => {
                                dados.push({
                                    data: f.data,
                                    banco: t.bancoNome,
                                    valor: t.valor,
                                    centro: f.centroCusto
                                });
                            });
                        }
                    });
                    
                    if (centro) dados = dados.filter(d => d.centro === centro);
                    if (mes) {
                        const [ano, mesNum] = mes.split('-');
                        dados = dados.filter(d => {
                            const data = new Date(d.data);
                            return data.getFullYear() == ano && (data.getMonth() + 1) == mesNum;
                        });
                    }
                    break;
                    
                case 'despesas':
                    titulo = 'Relatório de Despesas';
                    headers = ['Data', 'Tipo', 'Descrição', 'Valor', 'Centro'];
                    
                    dados = [];
                    const fechamentosD = filterByUserCentro(database.fechamentosDiarios);
                    fechamentosD.forEach(f => {
                        if (f.despesas) {
                            f.despesas.forEach(d => {
                                dados.push({
                                    data: f.data,
                                    tipo: d.tipo,
                                    descricao: d.descricao,
                                    valor: d.valor,
                                    centro: f.centroCusto
                                });
                            });
                        }
                    });
                    
                    if (centro) dados = dados.filter(d => d.centro === centro);
                    if (mes) {
                        const [ano, mesNum] = mes.split('-');
                        dados = dados.filter(d => {
                            const data = new Date(d.data);
                            return data.getFullYear() == ano && (data.getMonth() + 1) == mesNum;
                        });
                    }
                    break;
                    
                default:
                    showAlert('Tipo de relatório não implementado.', 'warning');
                    return;
            }
            
            mostrarRelatorio(titulo, headers, dados, tipo);
        }
        
        function mostrarRelatorio(titulo, headers, dados, tipo) {
            const container = document.getElementById('report-result');
            if (!container) return;
            
            if (dados.length === 0) {
                container.innerHTML = '<div class="alert alert-warning">Nenhum dado encontrado para os filtros selecionados.</div>';
                return;
            }
            
            let html = `
                <h3 class="mb-3">${titulo}</h3>
                <table class="table">
                    <thead>
                        <tr>
                            ${headers.map(h => `<th>${h}</th>`).join('')}
                        </tr>
                    </thead>
                    <tbody>
            `;
            
            let totalGeral = 0;
            
            dados.forEach(item => {
                html += '<tr>';
                
                if (tipo === 'fechamentos') {
                    html += `
                        <td>${formatDate(item.data)}</td>
                        <td>${item.responsavel}</td>
                        <td>${item.centroCusto}</td>
                        <td>${formatCurrency(item.totalFaturamentos)}</td>
                        <td>${formatCurrency(item.totalDespesas)}</td>
                        <td>${formatCurrency(item.totalTaxas)}</td>
                        <td><strong>${formatCurrency(item.totalDiaria)}</strong></td>
                    `;
                    totalGeral += item.totalDiaria;
                } else if (tipo === 'convenios') {
                    html += `
                        <td>${item.nome}</td>
                        <td>${item.documento}</td>
                        <td>${item.tipo}</td>
                        <td>${item.centroCusto || 'Não vinculado'}</td>
                        <td><span class="badge ${item.status === 'Ativo' ? 'badge-success' : 'badge-danger'}">${item.status}</span></td>
                    `;
                } else if (tipo === 'taxas') {
                    html += `
                        <td>${formatDate(item.data)}</td>
                        <td>${item.banco}</td>
                        <td>${formatCurrency(item.valor)}</td>
                        <td>${item.centro}</td>
                    `;
                    totalGeral += item.valor;
                } else if (tipo === 'despesas') {
                    html += `
                        <td>${formatDate(item.data)}</td>
                        <td>${item.tipo}</td>
                        <td>${item.descricao}</td>
                        <td>${formatCurrency(item.valor)}</td>
                        <td>${item.centro}</td>
                    `;
                    totalGeral += item.valor;
                }
                
                html += '</tr>';
            });
            
            if (totalGeral > 0) {
                html += `
                    <tr style="border-top: 2px solid #333; font-weight: bold;">
                        <td colspan="${headers.length - 1}" class="text-right">Total Geral:</td>
                        <td>${formatCurrency(totalGeral)}</td>
                    </tr>
                `;
            }
            
            html += `
                    </tbody>
                </table>
                <div class="mt-3 text-right">
                    <button class="btn btn-primary" onclick="window.imprimirRelatorio()">
                        <i class="fas fa-print"></i> Imprimir Relatório
                    </button>
                </div>
            `;
            
            container.innerHTML = html;
        }
        
        function imprimirRelatorio() {
            window.print();
        }
        
        function loadConfiguracoesContent() {
            const page = document.getElementById('configuracoes');
            if (!page) return;
            
            if (currentUser.role !== "Administrador") {
                page.innerHTML = `
                    <div class="restricted-access">
                        <i class="fas fa-ban"></i>
                        <h3>Acesso Restrito</h3>
                        <p>Você não tem permissão para acessar as configurações do sistema.</p>
                    </div>
                `;
                return;
            }
            
            page.innerHTML = `
                <div class="form-container">
                    <h3 class="form-title">Configurações do Sistema</h3>
                    
                    <div class="form-group">
                        <label class="form-label" for="system-name">Nome do Sistema</label>
                        <input type="text" class="form-control" id="system-name" value="${database.settings.systemName}">
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
                    
                    <div class="form-group">
                        <button class="btn btn-primary" id="manage-logo-btn" style="width: 100%;">
                            <i class="fas fa-image"></i> Gerenciar Logomarca
                        </button>
                    </div>
                    
                    <button class="btn btn-primary btn-block" id="save-settings">
                        <i class="fas fa-save"></i> Salvar Configurações
                    </button>
                </div>
            `;
            
            const saveSettings = document.getElementById('save-settings');
            if (saveSettings) {
                saveSettings.addEventListener('click', function() {
                    saveSettingsFunc();
                });
            }
            
            const manageLogoBtn = document.getElementById('manage-logo-btn');
            if (manageLogoBtn) {
                manageLogoBtn.addEventListener('click', function() {
                    showLogoModal();
                });
            }
        }
        
        function saveSettingsFunc() {
            const systemName = document.getElementById('system-name')?.value;
            const sessionTimeout = parseInt(document.getElementById('session-timeout')?.value);
            const emailNotifications = document.getElementById('email-notifications')?.checked;
            const backupAutomatico = document.getElementById('backup-automatico')?.checked;
            
            if (systemName) database.settings.systemName = systemName;
            if (sessionTimeout) database.settings.sessionTimeout = sessionTimeout;
            if (emailNotifications !== undefined) database.settings.emailNotifications = emailNotifications;
            if (backupAutomatico !== undefined) database.settings.backupAutomatico = backupAutomatico;
            
            if (sessionTimer) {
                clearTimeout(sessionTimer);
                startSessionTimer();
            }
            
            document.title = `${database.settings.systemName} | Sistema de Gerenciamento`;
            
            showAlert('Configurações salvas com sucesso!', 'success');
        }
        
        function getCategoriaClass(categoria) {
            const classes = {
                'Despesa': 'badge-danger',
                'Receita': 'badge-success',
                'Investimento': 'badge-primary',
                'Manutenção': 'badge-warning'
            };
            return classes[categoria] || 'badge-secondary';
        }

        // Tornar funções globais para uso em eventos onclick
        window.removerConvenio = removerConvenio;
        window.removerTaxa = removerTaxa;
        window.removerDespesa = removerDespesa;
        window.editConvenio = editConvenio;
        window.toggleConvenioStatus = toggleConvenioStatus;
        window.editUsuario = editUsuario;
        window.toggleUsuarioStatus = toggleUsuarioStatus;
        window.visualizarFechamento = visualizarFechamento;
        window.imprimirFechamento = imprimirFechamento;
        window.visualizarLancamento = visualizarLancamento;
        window.imprimirRelatorio = imprimirRelatorio;
        
        // Funções de filtro do dashboard
        window.aplicarFiltrosDashboard = aplicarFiltrosDashboard;
        window.limparFiltrosDashboard = limparFiltrosDashboard;
        window.removerFiltro = removerFiltro;
    </script>
</body>
</html>
