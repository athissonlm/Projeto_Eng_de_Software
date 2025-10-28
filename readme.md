# Relatório do Projeto

**Disciplina:** Engenharia de Software — Projeto de Microserviços (2025/1)  
**Autor do Repositório analisado:** athissonlm / RafaelSaleme / caiojcavalcante 
**Repositório analisado:** Projeto_Eng_de_Software 
**Data da análise:** 28 de outubro de 2025
---

## 1. Enunciado do Projeto Baseado em Microserviços
O repositório analisado contém uma aplicação web desenvolvida em Python/Flask cuja finalidade é demonstrativa e pedagógica: prover funcionalidades de listagem e busca sobre um catálogo de produtos artesanais e sobre um acervo de músicas, com exemplos de ordenação (bubble sort), busca linear e uma busca 'binária' adaptada.

## Parte 1 - Modelagem

### 2. Metodologia Ágil utilizada
**Metodologia proposta : Scrum.**
### 3. Técnicas de Reuso de Software utilizadas
- Componentização via templates Jinja2.
- Reuso de dados por JSON estático.
- Módulos Python reutilizáveis.
### 4. Desenvolvimento segundo a Metodologia Ágil e Técnicas de Reuso
Resumo formalizado para relatório: Planejamento do Sprint, Sprints quinzenais, Daily Standups, Revisões.
### 5. Backlogs (Product Backlog e Sprint Backlogs)
#### Product Backlog (prioridade decrescente)
- Página inicial e navegação entre áreas (homepage).
- Lista de produtos (carregamento de produtos.json).
- Página de produto individual.
- Lista de músicas com player (carregamento de musicas.json).
- Implementação de algoritmos de ordenação (bubble sort) e de busca.
- Sistema de busca com opções de método (linear/binária) e ordenação.
- Refatoração para arquitetura de microserviços (proposta).
- Documentação do projeto e README.
- Deploy básico (Heroku / Render / Docker).
#### Sprint 1 (setup e páginas básicas)
- Criar estrutura Flask (main.py, views.py, templates).
- Implementar homepage e navegação.
- Carregar arquivos estáticos e templates.
#### Sprint 2 (produtos e músicas)
- Implementar leitura de produtos.json e musicas.json.
- Criar páginas de listagem e páginas individuais de produto.
- Adicionar player de música simples.
#### Sprint 3 (busca e ordenação)
- Implementar funções de ordenação e busca.
- Integrar parâmetros de query na UI.
- Testes manuais e correções de interface.
### 6. Arquitetura do sistema (Microservices)
#### 6.0 Estado atual
O estado atual do repositório é monolítico. Arquivos principais: main.py, views.py, models.py, templates/ e static/.
#### 6.1 Arquitetura proposta utilizando Microserviços (diagrama lógico - descrição)
Proponho decompor em: product-service, music-service, search-service, api-gateway, recommendation-service (opcional).
Diagrama lógico: Client --> API Gateway --> { product, music, search, recommendation }.
#### 6.2 API Gateway (código documentado)
No repositório atual não existe uma API Gateway. Como substituição, views.py atua como camada que agrega dados e serve templates.
Exemplo de endpoint no API Gateway (pseudo-código):
```
@app.route("/api/products/search")
def gateway_products_search():
    q = request.args.get("q", "")
    resp = requests.get(f"http://product-service/products/search?q={q}")
    return jsonify(resp.json())
```
#### 6.3 Especificação e código de cada microserviço
(A) Elementos reais (módulos/componentes no monolito)
- main.py — inicia a aplicação Flask.
- views.py — define rotas.
- models.py — carrega JSONs e algoritmos.
- templates/ — Jinja2 templates.
- static/ — assets e JSONs.
(B) Especificação proposta para cada microserviço (individualmente)
1. Product-service
- Responsabilidade: CRUD e consultas sobre produtos.
- Endpoints: GET /products, GET /products/{id}, GET /products/search?q=
- Tecnologias sugeridas: Python + FastAPI/Flask, SQLite/Postgres.
2. Music-service
- Responsabilidade: metadados de áudio e streaming.
- Endpoints: GET /tracks, GET /tracks/{id}, GET /tracks/search?q=
3. Search-service
- Responsabilidade: execução de algoritmos de busca e ordenação.
- Endpoint: POST /search (payload com domain, term, method, sort).
4. Recommendation-service (opcional)
- Responsabilidade: recomendações por similaridade textual (TF-IDF ou heurística).
#### 6.4 Especificação do sistema de recomendação
No repositório não existe recomendador. Proposta: TF-IDF + similaridade de cosseno, endpoint GET /recommendations/products/{id}.
#### 6.5 Tela mostrando a listagem de todos os microservices em execução
No ambiente local não há múltiplos processos. Em um setup multi-container use `docker ps` ou `kubectl get pods`.
---

## Parte 2 - Implementação

### 7. Tecnologias utilizadas na implementação do sistema (estado atual)
- Linguagem: Python 3.x
- Framework: Flask 3.1.2
- Dependências: requirements.txt
- Banco de dados: JSON estático (static/*.json)

### 8. Demonstração passo a passo das telas e funcionamento do sistema
1. Homepage (/)
2. Crafts (/crafts)
3. Produto (/produto/<name>)
4. Musicas (/musicas)
### 9. Código no GitHub
O código está presente no ZIP entregue. Para executar localmente:
```
pip install -r requirements.txt
python main.py
```
### 10. Referências bibliográficas
- Flask Documentation
- Scikit-learn (TF-IDF)
- Scrum Guide
---