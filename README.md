[AI Agent (RAG)_0928_lhm.json](https://github.com/user-attachments/files/32734496/AI.Agent.RAG._0928_lhm.json)
# 0928{
  "name": "AI Agent (RAG)_0928_lhm",
  "nodes": [
    {
      "parameters": {
        "pollTimes": {
          "item": [
            {
              "mode": "everyMonth"
            }
          ]
        },
        "triggerOn": "specificFile",
        "fileToWatch": {
          "__rl": true,
          "value": "1dW2ch2oRk2eQt_H5StZFE1ERlg8o25UF",
          "mode": "list",
          "cachedResultName": "02_AI Workflow_n8n.pdf",
          "cachedResultUrl": "https://drive.google.com/file/d/1dW2ch2oRk2eQt_H5StZFE1ERlg8o25UF/view?usp=drivesdk"
        }
      },
      "type": "n8n-nodes-base.googleDriveTrigger",
      "typeVersion": 1,
      "position": [
        0,
        0
      ],
      "id": "f00d1761-2fe9-4717-aa04-bf26234a8b6e",
      "name": "Google Drive Trigger",
      "credentials": {
        "googleDriveOAuth2Api": {
          "id": "UT9ERWhISH77BLyx",
          "name": "Google Drive account"
        }
      }
    },
    {
      "parameters": {
        "operation": "download",
        "fileId": {
          "__rl": true,
          "value": "={{ $json.id }}",
          "mode": "id"
        },
        "options": {}
      },
      "type": "n8n-nodes-base.googleDrive",
      "typeVersion": 3,
      "position": [
        256,
        0
      ],
      "id": "faa4b88b-473c-4fa5-a3bd-17a749fc1eda",
      "name": "Download file",
      "credentials": {
        "googleDriveOAuth2Api": {
          "id": "UT9ERWhISH77BLyx",
          "name": "Google Drive account"
        }
      }
    },
    {
      "parameters": {
        "mode": "insert",
        "tableName": {
          "__rl": true,
          "mode": "list",
          "value": "documents"
        },
        "options": {}
      },
      "type": "@n8n/n8n-nodes-langchain.vectorStoreSupabase",
      "typeVersion": 1.3,
      "position": [
        512,
        0
      ],
      "id": "ecf98c65-9617-4cbc-a6bd-953c55379dee",
      "name": "Supabase Vector Store",
      "credentials": {
        "supabaseApi": {
          "id": "2PebmOFkhOXM0feC",
          "name": "Supabase account"
        }
      }
    },
    {
      "parameters": {},
      "type": "@n8n/n8n-nodes-langchain.embeddingsGoogleGemini",
      "typeVersion": 1,
      "position": [
        352,
        256
      ],
      "id": "aca31bd5-846f-4112-9326-eafc92815287",
      "name": "Embeddings Google Gemini",
      "credentials": {
        "googlePalmApi": {
          "id": "iWDwn9hSfbQYyLLf",
          "name": "Google Gemini(PaLM) Api account"
        }
      }
    },
    {
      "parameters": {
        "dataType": "binary",
        "textSplittingMode": "custom",
        "options": {
          "metadata": {
            "metadataValues": [
              {
                "name": "=document_title",
                "value": "={{ $json.name }}"
              }
            ]
          }
        }
      },
      "type": "@n8n/n8n-nodes-langchain.documentDefaultDataLoader",
      "typeVersion": 1.1,
      "position": [
        624,
        256
      ],
      "id": "784c7546-043c-4e11-9944-fee5dd8e0ce9",
      "name": "Default Data Loader"
    },
    {
      "parameters": {
        "options": {}
      },
      "type": "@n8n/n8n-nodes-langchain.textSplitterRecursiveCharacterTextSplitter",
      "typeVersion": 1,
      "position": [
        544,
        448
      ],
      "id": "ac06ea14-56c2-4b07-8b37-857b2bdc3315",
      "name": "Recursive Character Text Splitter"
    }
  ],
  "pinData": {},
  "connections": {
    "Google Drive Trigger": {
      "main": [
        [
          {
            "node": "Download file",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Download file": {
      "main": [
        [
          {
            "node": "Supabase Vector Store",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Embeddings Google Gemini": {
      "ai_embedding": [
        [
          {
            "node": "Supabase Vector Store",
            "type": "ai_embedding",
            "index": 0
          }
        ]
      ]
    },
    "Default Data Loader": {
      "ai_document": [
        [
          {
            "node": "Supabase Vector Store",
            "type": "ai_document",
            "index": 0
          }
        ]
      ]
    },
    "Recursive Character Text Splitter": {
      "ai_textSplitter": [
        [
          {
            "node": "Default Data Loader",
            "type": "ai_textSplitter",
            "index": 0
          }
        ]
      ]
    }
  },
  "active": false,
  "settings": {
    "executionOrder": "v1",
    "binaryMode": "separate"
  },
  "versionId": "73393a83-94e5-4295-9a0a-6c04da7bb2e5",
  "meta": {
    "templateCredsSetupCompleted": true,
    "instanceId": "e3b06985d6a6ed982a7b5886fb1f4ac0b0e75296b6ceb4745412a865e0c9f88a"
  },
  "nodeGroups": [],
  "id": "9DShFpeLxMuQ0vCF",
  "tags": []
}
