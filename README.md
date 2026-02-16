{
  "nodes": [
    {
      "parameters": {
        "documentId": {
          "__rl": true,
          "value": "1aRWrw6TQtgRW4jtLvmXJIyJQ7DmPcD2eeyAnpZGFE6o",
          "mode": "list",
          "cachedResultName": "Planilha sem título",
          "cachedResultUrl": "https://docs.google.com/spreadsheets/d/1aRWrw6TQtgRW4jtLvmXJIyJQ7DmPcD2eeyAnpZGFE6o/edit?usp=drivesdk"
        },
        "sheetName": {
          "__rl": true,
          "value": "gid=0",
          "mode": "list",
          "cachedResultName": "Aba1",
          "cachedResultUrl": "https://docs.google.com/spreadsheets/d/1aRWrw6TQtgRW4jtLvmXJIyJQ7DmPcD2eeyAnpZGFE6o/edit#gid=0"
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
      "position": [
        16,
        1008
      ],
      "id": "ca441988-58ee-4025-ad6a-0ac049a25c1c",
      "name": "1. Buscar Tema na Planilha1",
      "credentials": {
        "googleSheetsOAuth2Api": {
          "id": "HpaJQgK9yT8xa4Kh",
          "name": "Google Sheets account"
        }
      }
    },
    {
      "parameters": {
        "documentId": {
          "__rl": true,
          "value": "1aRWrw6TQtgRW4jtLvmXJIyJQ7DmPcD2eeyAnpZGFE6o",
          "mode": "list",
          "cachedResultName": "Planilha sem título",
          "cachedResultUrl": "https://docs.google.com/spreadsheets/d/1aRWrw6TQtgRW4jtLvmXJIyJQ7DmPcD2eeyAnpZGFE6o/edit?usp=drivesdk"
        },
        "sheetName": {
          "__rl": true,
          "value": 38111464,
          "mode": "list",
          "cachedResultName": "Aba2",
          "cachedResultUrl": "https://docs.google.com/spreadsheets/d/1aRWrw6TQtgRW4jtLvmXJIyJQ7DmPcD2eeyAnpZGFE6o/edit#gid=38111464"
        },
        "options": {}
      },
      "type": "n8n-nodes-base.googleSheets",
      "typeVersion": 4.7,
      "position": [
        176,
        1008
      ],
      "id": "8516012f-63f8-40f6-ba08-c5f078b3b525",
      "name": "2. Buscar Produto Afiliado1",
      "credentials": {
        "googleSheetsOAuth2Api": {
          "id": "HpaJQgK9yT8xa4Kh",
          "name": "Google Sheets account"
        }
      }
    },
    {
      "parameters": {
        "aggregate": "aggregateAllItemData",
        "options": {}
      },
      "type": "n8n-nodes-base.aggregate",
      "typeVersion": 1,
      "position": [
        368,
        1008
      ],
      "id": "ea2b473e-7e55-41de-8b74-b07e2b9ec1be",
      "name": "3. Agregar Dados da Planilha1"
    },
    {
      "parameters": {
        "method": "POST",
        "url": "https://minibilingue.com.br/wp-json/wp/v2/posts",
        "authentication": "genericCredentialType",
        "genericAuthType": "httpBasicAuth",
        "sendBody": true,
        "bodyParameters": {
          "parameters": [
            {
              "name": "title",
              "value": "={{ $json.title }}"
            },
            {
              "name": "content",
              "value": "={{ $json.content }}"
            },
            {
              "name": "slug",
              "value": "={{ $json.slug }}"
            },
            {
              "name": "status",
              "value": "draft"
            }
          ]
        },
        "options": {}
      },
      "type": "n8n-nodes-base.httpRequest",
      "typeVersion": 4.3,
      "position": [
        1440,
        720
      ],
      "id": "38c5180f-999e-4c4e-bb56-3112c6019972",
      "name": "9. Criar Post Draft no WP1",
      "credentials": {
        "httpBasicAuth": {
          "id": "97kZihf7s7jflaKA",
          "name": "Unnamed credential"
        }
      }
    },
    {
      "parameters": {
        "rule": {
          "interval": [
            {
              "triggerAtHour": 8,
              "triggerAtMinute": 15
            },
            {
              "triggerAtHour": 14,
              "triggerAtMinute": 30
            },
            {
              "triggerAtHour": 22,
              "triggerAtMinute": 22
            }
          ]
        }
      },
      "type": "n8n-nodes-base.scheduleTrigger",
      "typeVersion": 1.3,
      "position": [
        -176,
        1008
      ],
      "id": "d63c9a92-1241-40d0-abec-a72d55c85aec",
      "name": "Schedule Trigger1"
    },
    {
      "parameters": {
        "operation": "update",
        "documentId": {
          "__rl": true,
          "value": "1aRWrw6TQtgRW4jtLvmXJIyJQ7DmPcD2eeyAnpZGFE6o",
          "mode": "list",
          "cachedResultName": "Planilha sem título",
          "cachedResultUrl": "https://docs.google.com/spreadsheets/d/1aRWrw6TQtgRW4jtLvmXJIyJQ7DmPcD2eeyAnpZGFE6o/edit?usp=drivesdk"
        },
        "sheetName": {
          "__rl": true,
          "value": "gid=0",
          "mode": "list",
          "cachedResultName": "Aba1",
          "cachedResultUrl": "https://docs.google.com/spreadsheets/d/1aRWrw6TQtgRW4jtLvmXJIyJQ7DmPcD2eeyAnpZGFE6o/edit#gid=0"
        },
        "columns": {
          "mappingMode": "defineBelow",
          "value": {
            "row_number": "={{ $node[\"1. Buscar Tema na Planilha1\"].json[\"row_number\"] }}",
            "Status": "Publicado"
          },
          "matchingColumns": [
            "row_number"
          ],
          "schema": [
            {
              "id": "ID",
              "displayName": "ID",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true,
              "removed": true
            },
            {
              "id": "Tema",
              "displayName": "Tema",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true,
              "removed": true
            },
            {
              "id": "Palavra-Chave",
              "displayName": "Palavra-Chave",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true,
              "removed": true
            },
            {
              "id": "Status",
              "displayName": "Status",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            },
            {
              "id": "row_number",
              "displayName": "row_number",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "number",
              "canBeUsedToMatch": true,
              "readOnly": true,
              "removed": false
            }
          ],
          "attemptToConvertTypes": false,
          "convertFieldsToString": false
        },
        "options": {}
      },
      "type": "n8n-nodes-base.googleSheets",
      "typeVersion": 4.7,
      "position": [
        1792,
        960
      ],
      "id": "d5f9e760-398a-4d55-9c52-ad4e12bedbaf",
      "name": "11. Marcar como Postado na Planilha1",
      "credentials": {
        "googleSheetsOAuth2Api": {
          "id": "HpaJQgK9yT8xa4Kh",
          "name": "Google Sheets account"
        }
      }
    },
    {
      "parameters": {
        "method": "POST",
        "url": "=https://minibilingue.com.br/wp-json/wp/v2/posts/{{ $node[\"9. Criar Post Draft no WP1\"].json.id }}",
        "authentication": "genericCredentialType",
        "genericAuthType": "httpBasicAuth",
        "sendBody": true,
        "specifyBody": "json",
        "jsonBody": "={\n  \"meta\": {\n    \"rank_math_focus_keyword\": \"{{ $node[\"Code: Unificado1\"].json.rank_math_focus_keyword }}\",\n    \"rank_math_title\": \"{{ $node[\"Code: Unificado1\"].json.title }}\",\n    \"rank_math_description\": \"{{ $node[\"Code: Unificado1\"].json.rank_math_description }}\"\n  }\n}",
        "options": {}
      },
      "type": "n8n-nodes-base.httpRequest",
      "typeVersion": 4.3,
      "position": [
        1600,
        720
      ],
      "id": "61d17cf8-6b31-4e92-9858-2fd9509daa22",
      "name": "Refresh Rank Math",
      "credentials": {
        "httpBasicAuth": {
          "id": "97kZihf7s7jflaKA",
          "name": "Unnamed credential"
        }
      }
    },
    {
      "parameters": {
        "jsCode": "try {\n  /* =========================\n     1. CONTEÚDO DO ARTIGO\n  ========================= */\n  const artigo = $json.content_html;\n\n  if (typeof artigo !== \"string\" || artigo.length < 200) {\n    throw new Error(\"Conteúdo HTML inválido\");\n  }\n\n  /* =========================\n     2. SEO DO LLM (NORMALIZAÇÃO)\n  ========================= */\n  const seoItems = $items(\"LLM 2: \");\n  if (!seoItems.length) {\n    throw new Error(\"Node SEO não executou\");\n  }\n\n  const rawSeo = seoItems[0].json;\n\n  // 🔑 pega a resposta real do LangChain\n  const possiblePayload =\n    rawSeo.text ||\n    rawSeo.output ||\n    rawSeo.response ||\n    rawSeo;\n\n  let seo;\n  try {\n    seo = typeof possiblePayload === \"string\"\n      ? JSON.parse(possiblePayload)\n      : possiblePayload;\n  } catch {\n    throw new Error(\"SEO retornado não é JSON válido\");\n  }\n\n  if (!seo.title) {\n    throw new Error(\"Campo 'title' ausente após parse do SEO\");\n  }\n\n  /* =========================\n     3. SLUG BLINDADO\n  ========================= */\n  const slugBase = seo.slug || seo.title;\n\n  const slug = slugBase\n    .normalize(\"NFD\")\n    .replace(/[\\u0300-\\u036f]/g, \"\")\n    .replace(/[^a-z0-9-]/gi, \"\")\n    .toLowerCase();\n\n  /* =========================\n     4. PLANILHA\n  ========================= */\n  const planilha = $items(\"1. Buscar Tema na Planilha1\")[0]?.json || {};\n\n  /* =========================\n     5. SAÍDA FINAL WORDPRESS\n  ========================= */\n  return [\n    {\n      json: {\n        title: seo.title,\n        slug,\n        content: artigo,\n        status: \"draft\",\n        featured_media: planilha.featured_media || 0,\n        rank_math_focus_keyword: planilha[\"Palavra-Chave\"] || \"\",\n        rank_math_description: seo.meta_description || \"\"\n      }\n    }\n  ];\n\n} catch (e) {\n  throw new Error(\"Erro no Code Node Unificado → \" + e.message);\n}\n"
      },
      "type": "n8n-nodes-base.code",
      "typeVersion": 2,
      "position": [
        992,
        720
      ],
      "id": "adae5524-2290-41cd-a958-d4e8a949248b",
      "name": "Code: Unificado1"
    },
    {
      "parameters": {
        "jsCode": "try {\n  /* =========================\n     1. CONTEÚDO DO ARTIGO\n  ========================= */\n  const artigo = $json.content_html;\n\n  if (typeof artigo !== \"string\" || artigo.length < 200) {\n    throw new Error(\"Conteúdo HTML inválido\");\n  }\n\n  /* =========================\n     2. SEO DO LLM (NORMALIZAÇÃO)\n  ========================= */\n  const seoItems = $items(\"LLM 1: Gerador de SEO\");\n  if (!seoItems.length) {\n    throw new Error(\"Node SEO não executou\");\n  }\n\n  const rawSeo = seoItems[0].json;\n\n  // 🔑 pega a resposta real do LangChain\n  const possiblePayload =\n    rawSeo.text ||\n    rawSeo.output ||\n    rawSeo.response ||\n    rawSeo;\n\n  let seo;\n  try {\n    seo = typeof possiblePayload === \"string\"\n      ? JSON.parse(possiblePayload)\n      : possiblePayload;\n  } catch {\n    throw new Error(\"SEO retornado não é JSON válido\");\n  }\n\n  if (!seo.title) {\n    throw new Error(\"Campo 'title' ausente após parse do SEO\");\n  }\n\n  /* =========================\n     3. SLUG BLINDADO\n  ========================= */\n  const slugBase = seo.slug || seo.title;\n\n  const slug = slugBase\n    .normalize(\"NFD\")\n    .replace(/[\\u0300-\\u036f]/g, \"\")\n    .replace(/[^a-z0-9-]/gi, \"\")\n    .toLowerCase();\n\n  /* =========================\n     4. PLANILHA\n  ========================= */\n  const planilha = $items(\"1. Buscar Tema na Planilha\")[0]?.json || {};\n\n  /* =========================\n     5. SAÍDA FINAL WORDPRESS\n  ========================= */\n  return [\n    {\n      json: {\n        title: seo.title,\n        slug,\n        content: artigo,\n        status: \"draft\",\n        featured_media: planilha.featured_media || 0,\n        rank_math_focus_keyword: planilha[\"Palavra-Chave\"] || \"\",\n        rank_math_description: seo.meta_description || \"\"\n      }\n    }\n  ];\n\n} catch (e) {\n  throw new Error(\"Erro no Code Node Unificado → \" + e.message);\n}\n"
      },
      "type": "n8n-nodes-base.code",
      "typeVersion": 2,
      "position": [
        1232,
        720
      ],
      "id": "42bc28e6-1dee-4240-9750-4716ac1499f7",
      "name": "Code: Unificado2"
    },
    {
      "parameters": {
        "assignments": {
          "assignments": [
            {
              "id": "61ab2507-db25-44eb-9b0a-f5f5654b043e",
              "name": "content_html",
              "value": "=={{ $json.html?.content || $json.html || $json.data || \"\" }}\n",
              "type": "string"
            }
          ]
        },
        "options": {}
      },
      "type": "n8n-nodes-base.set",
      "typeVersion": 3.4,
      "position": [
        992,
        1008
      ],
      "id": "e14897d2-8e4d-4242-959f-d21a11028b78",
      "name": "Edit Fields2"
    },
    {
      "parameters": {
        "conditions": {
          "options": {
            "caseSensitive": true,
            "leftValue": "",
            "typeValidation": "strict",
            "version": 3
          },
          "conditions": [
            {
              "id": "8e9b9fa7-ac08-4a4d-9bc9-a65f81acc2a3",
              "leftValue": "={{ $('LLM 1').item.json.text }}\n",
              "rightValue": "",
              "operator": {
                "type": "string",
                "operation": "notEmpty",
                "singleValue": true
              }
            },
            {
              "id": "bdc367be-80e3-4f13-a51a-bcbb14c692dd",
              "leftValue": "={{$json.text.length}}\n",
              "rightValue": 600,
              "operator": {
                "type": "number",
                "operation": "gte"
              }
            }
          ],
          "combinator": "and"
        },
        "options": {}
      },
      "type": "n8n-nodes-base.if",
      "typeVersion": 2.3,
      "position": [
        1408,
        1008
      ],
      "id": "e01a78d9-7431-4b89-b823-f57467423370",
      "name": "If1"
    },
    {
      "parameters": {
        "promptType": "define",
        "text": "=Você é Paulo, Pedagogo Bilíngue e Especialista em SEO técnico para WordPress.\n\nSua tarefa é gerar APENAS metadados técnicos de SEO.\n\n========================\nDADOS DE ENTRADA\n========================\nTEMA: {{ $('1. Buscar Tema na Planilha').item.json.Tema }}\nPALAVRA_CHAVE: {{ $('1. Buscar Tema na Planilha').item.json['Palavra-Chave'] }}\n\n========================\nREGRAS OBRIGATÓRIAS\n========================\n1. Responda EXCLUSIVAMENTE com um objeto JSON válido.\n2. NÃO escreva explicações, comentários ou texto fora do JSON.\n3. NÃO use markdown.\n4. NÃO use blocos de código.\n5. Use apenas aspas duplas padrão (\").\n6. NÃO inclua campos extras.\n7. O JSON deve começar com { e terminar com }.\n8. Retorne o JSON como objeto direto, não como string.\n\n========================\nDIRETRIZES DE CONTEÚDO\n========================\n\ntitle:\n- Deve funcionar como H1\n- Fórmula obrigatória:\n  [Desejo do Pai/Mãe] + [Benefício Cognitivo Real] + [PALAVRA_CHAVE]\n\nslug:\n- Use SOMENTE a PALAVRA_CHAVE\n- Minúsculas\n- Palavras separadas por hífen\n- Sem acent\n",
        "batching": {}
      },
      "type": "@n8n/n8n-nodes-langchain.chainLlm",
      "typeVersion": 1.9,
      "position": [
        656,
        720
      ],
      "id": "811b4e21-51b1-44f2-a9a5-3cf8be99b43b",
      "name": "LLM 2: ",
      "alwaysOutputData": true,
      "onError": "continueRegularOutput"
    },
    {
      "parameters": {
        "promptType": "define",
        "text": "=Você é Paulo, pedagogo bilíngue especialista em educação infantil e ensino de inglês para crianças.\n\nSua tarefa é escrever UM ARTIGO EDUCATIVO para pais e mães.\n\n========================\nREGRAS OBRIGATÓRIAS\n========================\n\n1. Gere o conteúdo em HTML válido.\n2. Use EXATAMENTE:\n   - 1 <h1>\n   - Múltiplos <h2>\n3. NÃO escreva títulos como texto solto.\n4. NÃO use rótulos genéricos como \"Gancho\", \"Ponte\", \"Curadoria\", \"Introdução\".\n5. Todo texto deve estar dentro de <p>.\n6. Cada <h2> deve explicar o conteúdo do parágrafo seguinte.\n7. Mínimo de 600 palavras.\n8. Tom acolhedor, prático e pedagógico.\n\n========================\nESTRUTURA OBRIGATÓRIA\n========================\n\n<h1>Título principal claro e direto</h1>\n\n<h2>Contexto real entre pais e filhos</h2>\n<p>Texto...</p>\n\n<h2>Base pedagógica e cognitiva</h2>\n<p>Texto...</p>\n\n<h2>Atividades práticas em casa</h2>\n<p>Texto...</p>\n\n<h2>Dicas finais para os pais</h2>\n<p>Texto...</p>\n\n========================\nDADOS DE ENTRADA\n========================\n\nTema: {{ $json.data[0].Tema }}\nPalavra-chave principal: {{ $json.data[0]['Palavra-Chave'] }}\n\n========================\nIMPORTANTE\n========================\n- Responda SOMENTE com HTML.\n- Não use markdown.\n- Não escreva explicações fora do conteúdo.\n",
        "batching": {}
      },
      "type": "@n8n/n8n-nodes-langchain.chainLlm",
      "typeVersion": 1.9,
      "position": [
        624,
        1008
      ],
      "id": "2d2cc87f-6cad-46a5-a650-08bcb80ae050",
      "name": "LLM 1"
    },
    {
      "parameters": {
        "options": {
          "maxOutputTokens": 2048,
          "temperature": 0.6
        }
      },
      "type": "@n8n/n8n-nodes-langchain.lmChatGoogleGemini",
      "typeVersion": 1,
      "position": [
        544,
        1312
      ],
      "id": "eaaa2353-c7fb-4b47-b58d-3aa04a7441ad",
      "name": "Google Gemini Chat Model1",
      "retryOnFail": true,
      "credentials": {
        "googlePalmApi": {
          "id": "hN0GvgQJvRBt4Mpt",
          "name": "Google Gemini(PaLM) Api account 2"
        }
      }
    },
    {
      "parameters": {
        "model": "llama-3.3-70b-versatile",
        "options": {
          "maxTokensToSample": 4500,
          "temperature": 0.7
        }
      },
      "type": "@n8n/n8n-nodes-langchain.lmChatGroq",
      "typeVersion": 1,
      "position": [
        720,
        896
      ],
      "id": "4afe57cf-cd45-423b-8538-8b7215576c66",
      "name": "Groq Chat Model1",
      "credentials": {
        "groqApi": {
          "id": "7TzsNlJUXsW1my1X",
          "name": "Groq account"
        }
      }
    },
    {
      "parameters": {
        "jsCode": "if (typeof $json.content_html !== 'string') {\n  throw new Error('content_html não é string');\n}\n\nif ($json.content_html.length < 200) {\n  throw new Error('HTML muito curto');\n}\n\nreturn [{ json: $json }];\n"
      },
      "type": "n8n-nodes-base.code",
      "typeVersion": 2,
      "position": [
        1200,
        1008
      ],
      "id": "e071e711-2172-4765-b1ca-d7ec4ae86763",
      "name": "Code in JavaScript1"
    }
  ],
  "connections": {
    "1. Buscar Tema na Planilha1": {
      "main": [
        [
          {
            "node": "2. Buscar Produto Afiliado1",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "2. Buscar Produto Afiliado1": {
      "main": [
        [
          {
            "node": "3. Agregar Dados da Planilha1",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "3. Agregar Dados da Planilha1": {
      "main": [
        [
          {
            "node": "LLM 1",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "9. Criar Post Draft no WP1": {
      "main": [
        [
          {
            "node": "Refresh Rank Math",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Schedule Trigger1": {
      "main": [
        [
          {
            "node": "1. Buscar Tema na Planilha1",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Refresh Rank Math": {
      "main": [
        [
          {
            "node": "11. Marcar como Postado na Planilha1",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Code: Unificado1": {
      "main": [
        [
          {
            "node": "Code: Unificado2",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Code: Unificado2": {
      "main": [
        [
          {
            "node": "9. Criar Post Draft no WP1",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Edit Fields2": {
      "main": [
        [
          {
            "node": "Code in JavaScript1",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "If1": {
      "main": [
        [
          {
            "node": "LLM 2: ",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "LLM 2: ": {
      "main": [
        [
          {
            "node": "Code: Unificado1",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "LLM 1": {
      "main": [
        [
          {
            "node": "Edit Fields2",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Google Gemini Chat Model1": {
      "ai_languageModel": [
        [
          {
            "node": "LLM 1",
            "type": "ai_languageModel",
            "index": 0
          }
        ]
      ]
    },
    "Groq Chat Model1": {
      "ai_languageModel": [
        [
          {
            "node": "LLM 2: ",
            "type": "ai_languageModel",
            "index": 0
          }
        ]
      ]
    },
    "Code in JavaScript1": {
      "main": [
        [
          {
            "node": "If1",
            "type": "main",
            "index": 0
          }
        ]
      ]
    }
  },
  "pinData": {},
  "meta": {
    "templateCredsSetupCompleted": true,
    "instanceId": "15e5e4d98fce5be652f67c4482f3751ad200fedbc3d8da2c3a64063a7f1f5f49"
  }
}
