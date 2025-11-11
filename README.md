🧭 1. Criar o projeto no Supabase

Vá em https://supabase.com
 e crie uma conta.

Clique em New Project → escolha nome, senha do banco e região.

Após criar, você verá o painel do projeto (Dashboard).

🧩 2. Criar a tabela usada no editor

Clique em Table Editor → New Table e defina:

Coluna	Tipo	Descrição
id	uuid (Primary Key, Default → uuid_generate_v4())	identificador único
novel	text	nome do projeto/visual novel
conteudo	jsonb	JSON completo do editor (projeto salvo)
created_at	timestamp (Default: now())	data de criação (opcional)

💡 Nome da tabela: por exemplo visual_novels.

⚠️ Importante:
Deixe as políticas RLS (Row Level Security) desativadas no início enquanto testa.
(Depois, se quiser proteger, posso te ajudar a criar políticas seguras para leitura pública e gravação privada.)

🧠 3. Configurar as variáveis no seu código

No início do seu script há estas linhas:

const SUPABASE_URL = "URL do seu SUPABASE";
const SUPABASE_KEY = "chave do seu supabase";
const TABLE_NAME = "nome da sua tabela";


Preencha com:

const SUPABASE_URL = "https://<seu-projeto>.supabase.co";
const SUPABASE_KEY = "<anon public key>";
const TABLE_NAME = "visual_novels";


Você encontra a anon public key no painel:
→ ⚙️ Settings → API → Project API Keys → anon public

🧾 4. Testar Salvamento e Leitura
Salvar

Clique em 💾 Salvar, e ele vai criar um registro na tabela com:

{
  "novel": "Meu Namorado é um Vampiro",
  "conteudo": { ...todo o JSON do projeto... }
}

Carregar

Ao clicar em 📂 Carregar, o script executa:

supabase.from(TABLE_NAME)
        .select("conteudo")
        .eq("novel", s)
        .order("id", { ascending: false })
        .limit(1)


Ou seja: busca o último projeto salvo com o mesmo título de novel.

🧰 5. Estrutura de dados esperada no conteudo (JSON)

O campo conteudo salva toda a novel neste formato:

{
  "projectTitle": "Meu Namorado é um Vampiro",
  "globals": {
    "variables": { "confiança": 0, "amor": 10 },
    "flags": { "viu_intro": true },
    "inventory": ["colar antigo"]
  },
  "characters": [
    { "name": "Luna", "expression": "feliz" },
    { "name": "Dimitri", "expression": "tímido" }
  ],
  "episodes": [
    {
      "id": "ep1",
      "title": "Capítulo 1 – O Encontro",
      "scenes": [
        {
          "id": "scene1",
          "title": "No Café",
          "dialogues": [
            {
              "character": "Luna",
              "expression": "neutra",
              "line": "Você vem sempre aqui?",
              "choices": [
                {
                  "text": "Sim, gosto desse lugar",
                  "condition": { "confiança": { "gte": 10 } },
                  "effects": { "amor": +5 },
                  "flags": { "viu_intro": true },
                  "next": 2
                }
              ]
            }
          ]
        }
      ]
    }
  ]
}

🧪 6. Teste rápido local

Salve o arquivo .html em uma pasta local.

Abra no navegador (duplo clique).

Preencha as credenciais Supabase.

Crie e salve um projeto — ele aparecerá automaticamente no select “novel”.

✅ 7. (Opcional) Melhorias futuras

Adicionar auth do Supabase para login e salvar cada projeto por usuário.

Criar tabela users e relacionar com visual_novels.

Criar função update() em vez de insert() (para sobrescrever saves antigos).

Habilitar RLS com política segura (auth.uid() = user_id).