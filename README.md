# 🔌 Controle de LED com ESP32 via MQTT v5

Este projeto implementa um cliente MQTT utilizando o protocolo MQTT v5 rodando no ESP32. O dispositivo se inscreve no tópico `/ifpe/ads/embarcados/esp32/led` para receber comandos que ligam ou desligam o LED onboard da placa.

---

## 🎯 Objetivo

- 💡 Quando o valor `1` é publicado no tópico MQTT, o LED acende.
- 💡 Quando o valor `0` é publicado, o LED apaga.

---

## 🛠️ Tecnologias Utilizadas

- ESP32 + Placa de desenvolvimento
- [ESP-IDF](https://docs.espressif.com/projects/esp-idf/en/latest/esp32/index.html)
- Protocolo MQTT v5
- Broker [HiveMQ Cloud](https://www.hivemq.com/mqtt-cloud-broker/) (com TLS)
- Extensão [ESP-IDF VSCode Extension](https://github.com/espressif/vscode-esp-idf-extension)
- Linguagem C

---

## 📦 Estrutura do Projeto

```
mqtt5/
├── main/
│ ├── app_main.c         # Código principal
│ ├── CMakeLists.txt
│ ├── idf_component.yml
│ └── Kconfig.projbuild
├── CMakeLists.txt
├── dependencies.lock
├── .gitignore
└── README.md            # Este arquivo
```

---

## 🚀 Como Executar o Projeto

### 1. Clone o repositório

```bash
git clone https://github.com/kerlenmelo/projeto-esp32-mqtt-client
cd projeto-esp32-mqtt-client
```

### 2. Configure o ambiente no VSCode com ESP-IDF

- Certifique-se de que a extensão **ESP-IDF** está instalada no VS Code
- Pressione `F1` e selecione `ESP-IDF: Configure ESP-IDF extension`
- Siga as instruções para instalação dos componentes (Python, toolchain, CMake etc.)
- Conecte o ESP32 à porta USB do seu computador

---

### 3. Configurar Wi-Fi

## Você pode configurar sua rede Wi-Fi de três formas:

#### ✅ Usando a extensão ESP-IDF no VSCode:

1. Com o projeto aberto no VS Code, clique no ícone da extensão **ESP-IDF** na barra lateral esquerda.
2. Selecione a opção **"SDK Configuration Editor"** (ou pressione `F1` e pesquise por `ESP-IDF: SDK Configuration Editor`).
3. Na janela de configurações que abrir:
   - Expanda a seção `Example Connection Configuration`
   - Insira o **SSID** (nome da sua rede Wi-Fi)
   - Insira a **senha** da sua rede
4. Clique em **Save** no canto superior direito da interface
5. A extensão irá atualizar automaticamente o arquivo `sdkconfig` com as configurações inseridas

#### ✅ Usando o menuconfig:

No terminal do VSCode:

```bash
idf.py menuconfig
```

- Navegue até: `Example Connection Configuration`
- Insira:
  - **SSID** da sua rede Wi-Fi
  - **Senha** da rede

#### 🛠️ Ou editando diretamente o arquivo `sdkconfig`:

Localize e altere as linhas:

```plaintext
CONFIG_EXAMPLE_WIFI_SSID="NOME_DA_SUA_REDE"
CONFIG_EXAMPLE_WIFI_PASSWORD="SENHA_DA_REDE"
```

---

### 4. Compile e grave o firmware

#### ✅ Usando a extensão ESP-IDF no VS Code

Você pode compilar, gravar e monitorar diretamente pela interface gráfica do VS Code com a extensão **ESP-IDF**:

- Utilize a barra lateral (ícone do ESP) ou a barra inferior para:
  - 🧱 Compilar: `Build Project`
  - 📥 Gravar: `Flash Device`
  - 🔍 Monitorar: `Monitor Device`
  - 🚀 Ou clique em `ESP-IDF: Build, Flash and Monitor` para executar tudo de uma vez

> ⚙️ Também é possível acessar essas funções pressionando `F1` e buscando por:
>
> - `ESP-IDF: Build Project`
> - `ESP-IDF: Flash Device`
> - `ESP-IDF: Monitor Device`

⚡ Certifique-se de que:

- O dispositivo ESP32 está conectado via USB
- A porta correta (ex: `COM3`) está selecionada no canto inferior da janela
- O target esteja definido como `esp32`

---

#### 💻 Alternativa via terminal

```bash
idf.py build
idf.py -p COMx flash monitor
```

Substitua `COMx` pela porta correspondente ao seu ESP32 (ex: COM3 no Windows ou /dev/ttyUSB0 no Linux).

---

## 📡 Testando com HiveMQ

### 1. Crie sua conta no HiveMQ Cloud

Acesse o site oficial do HiveMQ Cloud:

🔗 https://www.hivemq.com/mqtt-cloud-broker/

- Clique em **"Get Started Free"**
- Crie uma conta e confirme seu e-mail
- Crie uma instância gratuita do broker MQTT
- Após criada, acesse os detalhes da instância para obter:
  - **Hostname** (ex: `xxxxxxx.s1.eu.hivemq.cloud`)
  - **Porta** (geralmente `8883` para TLS)
  - **Usuário e Senha** que você mesmo define

---

### 2. Você pode testar sua aplicação de duas formas:

#### ✅ Acesse o Web Client do HiveMQ Cloud

Com a instância criada, clique na aba **"Web Client"** ou acesse diretamente:

🔗 `https://console.hivemq.cloud/clusters/<seu-cluster-id>/web-client`

Substitua `<seu-cluster-id>` pelo ID do seu cluster gerado no painel.

Preencha os campos de conexão:

- **Username**: o que você criou
- **Password**: a senha que você criou

Clique em **"Connect"**

Assine o tópico do LED:

- **TOPIC**: /ifpe/ads/embarcados/esp32/led
- **QOS**: 1

Clique em **"Subscribe"**

Envie a mensagem:

- **Message**:
- `1` (Acende o LED )
- `0` (Apaga o LED)
- **Topic**: /ifpe/ads/embarcados/esp32/led
- **QOS**: 1

Clique em **"Send Message"**

---

#### ✅ Usando o WebSocket Client da HiveMQ

Vá até o cliente de testes Web da HiveMQ:

🔗 https://www.hivemq.com/demos/websocket-client/

Preencha os campos com os dados da sua instância:

- **Host**: seu hostname gerado (ex: `xxxxx.s1.eu.hivemq.cloud`)
- **Port**: `8884` (WebSocket Port)
- **SSL**: ✅ Ativado
- **Username**: o que você criou
- **Password**: a senha que você criou
  Clique em **"Connect"**

---

No painel de Subscriptions:

- Clique em `Add New Topic Subscription`
- Insira o seguinte tópico: `/ifpe/ads/embarcados/esp32/led`

---

No campo de publicação (`Publish`):

- Tópico: `/ifpe/ads/embarcados/esp32/led`
- Mensagem:
  - `1` → Acende o LED
  - `0` → Apaga o LED
- QoS: 1
- Clique em **"Publish"**

---

## 💡 Resultado Esperado

Ao publicar os comandos MQTT (`1` ou `0`), o LED onboard da placa ESP32 deve acender ou apagar.

> ✅ Você também pode usar outros clientes MQTT, como **MQTT Explorer** ou **mosquitto_pub**, desde que se conectem ao seu broker com os dados corretos.

---
