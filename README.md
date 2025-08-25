# Crypto Assignment — Auditoria e Segurança

Implementação de **criptografia simétrica e assimétrica** em Python, com:
- **AES-256-GCM** (simétrica) + derivação de chave por **PBKDF2-HMAC-SHA256** a partir de senha.
- **RSA-3072 OAEP (SHA-256)** (assimétrica) em esquema **híbrido**: RSA protege a chave AES e AES-GCM protege os dados.
- Uma **CLI** (`cli.py`) para cifrar/decifrar arquivos ou texto.
- **Testes** básicos com `pytest`.

---

## Requisitos

- **Python 3.10+**
- `pip` (gerenciador de pacotes)
- (Opcional) Git para versionamento e envio ao GitHub

Instalação das dependências:
```bash
python -m pip install -r requirements.txt
```

---

## Arquivos principais

.
├── asymmetric.py     # RSA (chaves, encrypt/decrypt híbrido RSA+AES)
├── symmetric.py      # AES-GCM com PBKDF2 a partir de senha
├── cli.py            # Interface de linha de comando
├── requirements.txt
├── tests/
│   └── test_crypto.py
└── .gitignore

---

## Passo a passo (Ubuntu / Linux)

1. Criar e ativar um ambiente virtual (recomendado)

```bash
python3 -m venv .venv
source .venv/bin/activate
```

2. Instalar dependências

```bash
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
```

3. (Opcional) Rodar os testes

```bash
python -m pytest -q
```

>Se tudo certo, você verá os testes passando.

---

### Uso da CLI

A CLI possui dois modos:

- sym: simétrica (AES-GCM + PBKDF2 a partir de senha)
- rsa: assimétrica híbrida (RSA-OAEP + AES-GCM)

1. Criptografia Simétrica (AES-GCM + PBKDF2)

Cifrar arquivo com senha:

```bash
python cli.py sym encrypt --in arquivo.txt --out arquivo.txt.aes --password "minha-senha-forte"
```

Decifrar arquivo:

```bash
python cli.py sym decrypt --in arquivo.txt.aes --out arquivo.txt --password "minha-senha-forte"
```

Cifrar texto via stdin (saída em stdout):

```bash
echo "olá, mundo" | python cli.py sym encrypt --password "segredo" --out -
``` 

> Saída simétrica é um JSON contendo salt, nonce, ciphertext, tag (todos base64), além de metadados.

2. Criptografia Assimétrica Híbrida (RSA-OAEP + AES-GCM)

Gerar par de chaves (RSA 3072 bits):

```bash
python cli.py rsa gen-keys --private private_key.pem --public public_key.pem
```
> Você pode proteger a chave privada com senha adicionando --password "minha-senha-de-chave" no comando acima.

Cifrar com a chave pública:

```bash
python cli.py rsa encrypt --pub public_key.pem --in segredo.pdf --out segredo.enc.json
```

Decifrar com a chave privada:

```bash
python cli.py rsa decrypt --priv private_key.pem --in segredo.enc.json --out segredo.pdf
```

> Se a chave privada estiver protegida por senha, acrescente --password "minha-senha-de-chave" no comando rsa decrypt.

---

Alunos: Andriely Starosky e Wilson Hugen Junior