{
  "nodes": [
    {
      "parameters": {
        "documentId": {
          "__rl": true,
          "value": "ID_DA_SUA_PLANILHA_AQUI",
          "mode": "id"
        },
        "sheetName": {
          "__rl": true,
          "value": "Aba1",
          "mode": "name"
        },
        "filtersUI": {
          "values": [
            {
              "lookupColumn": "Status",
              "lookupValue": "Pendente"
            }
          ]
        },
        "options": {
          "returnFirstMatch": true
        }
      },
      "type": "n8n-nodes-base.googleSheets",
      "typeVersion": 4.7,
      "position": [0, 0],
      "id": "node-1",
      "name": "1. Buscar Pauta na Planilha",
      "credentials": {
        "googleSheetsOAuth2Api": {
          "id": "CREDENTIAL_ID",
          "name": "Google Sheets"
        }
      }
    },
    {
      "parameters": {
        "method": "POST",
        "url": "https://seu-site-wordpress.com.br/wp-json/wp/v2/posts",
        "authentication": "genericCredentialType",
        "genericAuthType": "httpBasicAuth",
        "sendBody": true,
        "bodyParameters": {
          "parameters": [
            { "name": "title", "value": "={{ $json.title }}" },
            { "name": "content", "value": "={{ $json.content }}" },
            { "name": "slug", "value": "={{ $json.slug }}" },
            { "name": "status", "value": "draft" }
          ]
        }
      },
      "type": "n8n-nodes-base.httpRequest",
      "typeVersion": 4.3,
      "position": [1400, -200],
      "id": "node-wp-post",
      "name": "9. Criar Post Draft no WP",
      "credentials": {
        "httpBasicAuth": {
          "id": "CREDENTIAL_ID",
          "name": "WordPress Auth"
        }
      }
    },
    {
      "parameters": {
        "jsCode": "try {\n  const artigo = $json.content_html;\n  if (!artigo || artigo.length < 200) throw new Error(\"HTML inválido\");\n\n  const seoItems = $items(\"LLM 2: \");\n  const rawSeo = seoItems[0].json;\n  const possiblePayload = rawSeo.text || rawSeo.output || rawSeo.response || rawSeo;\n\n  let seo = typeof possiblePayload === \"string\" ? JSON.parse(possiblePayload) : possiblePayload;\n\n  const slug = (seo.slug || seo.title)\n    .normalize(\"NFD\")\n    .replace(/[\\u0300-\\u036f]/g, \"\")\n    .replace(/[^a-z0-9-]/gi, \"\")\n    .toLowerCase();\n\n  const planilha = $items(\"1. Buscar Pauta na Planilha\")[0]?.json || {};\n\n  return [{\n    json: {\n      title: seo.title,\n      slug,\n      content: artigo,\n      status: \"draft\",\n      featured_media: planilha.featured_media || 0,\n      rank_math_focus_keyword: planilha[\"Palavra-Chave\"] || \"\",\n      rank_math_description: seo.meta_description || \"\"\n    }\n  }];\n} catch (e) {\n  throw new Error(\"Erro na unificação de dados: \" + e.message);\n}"
      },
      "type": "n8n-nodes-base.code",
      "typeVersion": 2,
      "position": [1000, -200],
      "id": "node-code-logic",
      "name": "Code: Normalização SEO"
    },
    {
      "parameters": {
        "promptType": "define",
        "text": " Você é um Especialista em SEO Técnico.\nGere metadados para o tema: {{ $json.Tema }}\nRetorne APENAS um JSON com os campos: title, slug, meta_description.",
        "batching": {}
      },
      "type": "@n8n/n8n-nodes-langchain.chainLlm",
      "typeVersion": 1.9,
      "position": [600, -200],
      "id": "node-llm-seo",
      "name": "LLM 2: Gerador de Metadados"
    },
    {
      "parameters": {
        "promptType": "define",
        "text": "Você é um Especialista em Conteúdo Bilíngue.\nEscreva um artigo educativo em HTML para o tema: {{ $json.Tema }}.\nUse H1, H2 e P. Mínimo 600 palavras.",
        "batching": {}
      },
      "type": "@n8n/n8n-nodes-langchain.chainLlm",
      "typeVersion": 1.9,
      "position": [600, 100],
      "id": "node-llm-content",
      "name": "LLM 1: Gerador de Artigo"
    }
  ],
  "connections": {
    "1. Buscar Pauta na Planilha": {
      "main": [[{ "node": "LLM 1: Gerador de Artigo", "type": "main", "index": 0 }]]
    },
    "LLM 1: Gerador de Artigo": {
      "main": [[{ "node": "LLM 2: Gerador de Metadados", "type": "main", "index": 0 }]]
    },
    "LLM 2: Gerador de Metadados": {
      "main": [[{ "node": "Code: Normalização SEO", "type": "main", "index": 0 }]]
    },
    "Code: Normalização SEO": {
      "main": [[{ "node": "9. Criar Post Draft no WP", "type": "main", "index": 0 }]]
    }
  }
}
