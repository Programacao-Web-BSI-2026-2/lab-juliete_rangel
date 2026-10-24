# lab-juliete_rangel
# Memória de Desenvolvimento — Semana 01: Protocolo HTTP & Sockets Bare-Metal
**Aluno:** Juliete Rangel de Mattos
**Matrícula:** 202411200320 
**Curso:** Sistemas de Informação  

---

## 1. Mapeamento da Camada de Transporte
Diagrama Mermaid demonstrando o ciclo de vida da requisição executada no laboratório:
## 1. Mapeamento da Camada de Transporte
Diagrama Mermaid demonstrando o ciclo de vida da requisição executada no laboratório:

sequenceDiagram
    autonumber
    participant C as Cliente (Netcat / cURL)
    participant S as Servidor HTTP

    C->>S: TCP SYN
    S-->>C: TCP SYN-ACK
    C->>S: TCP ACK (Socket Aberto)
    C->>S: GET / HTTP/1.1\r\nHost: example.com\r\n\r\n
    S-->>C: HTTP/1.1 200 OK (Payload + Headers)
    S->>C: TCP FIN
2. Registro dos Experimentos de Laboratório
Experimento 1: Netcat Nu & Falha 400 Bad Request
Comando Testado: nc -C example.com 80 sem o header Host:

Resultado: O servidor retorna um erro 400 Bad Request porque a especificação do HTTP/1.1 exige o cabeçalho Host para identificar qual site/domínio hospedar o pedido, permitindo o uso de Virtual Hosting (vários sites no mesmo endereço IP).

Experimento 2: Semântica dos Métodos HTTP
Análise dos Verbos: O método GET busca um recurso de forma segura e idempotente. O POST envia dados para serem processados (geralmente criando ou alterando recursos). O OPTIONS consulta quais opções de comunicação o servidor suporta para um determinado recurso.

Header Content-Length: Ele indica exatamente quantos bytes o servidor ou cliente deve ler no buffer do socket, informando o término do payload quando não se usa transferência chunked.

Experimento 3: URL, Query Strings & Percent-Encoding
URL Testada: (Passo 1) curl -v http://httpbin.org/get?curso=Engenharia da Computacao

URL Testada: (Passo 2) curl -v https://httpbin.org/get?curso=Engenharia%20da%20Computa%C3%A7%C3%A3o

O erro no Passo 1: O terminal ou o interpretador de comandos quebra a string por causa do espaço em branco não codificado, gerando erros de sintaxe ou interpretando como argumentos separados.

A função do %20: O percent-encoding substitui caracteres especiais e espaços por códigos hexadecimais iniciados por % para garantir que a URL seja transmitida de forma segura e interpretada sem ambiguidades pela camada de rede.

Experimento 4: Negociação de Conteúdo & User-Agent
Análise: O cabeçalho Accept diz ao servidor quais tipos de mídia o cliente prefere receber (ex: application/json, text/html), enquanto o User-Agent identifica o software, versão e sistema operacional do cliente, permitindo que o servidor adapte o conteúdo ou evite incompatibilidades.

Experimento 5: Redirecionamentos & Cache Condicional
Redirecionamento (301): Com a flag -L, o curl segue automaticamente o redirecionamento fornecido pelo servidor (ex: de HTTP para HTTPS). Sem a flag, ele apenas exibe a resposta de status 301/302 e os cabeçalhos de localização (Location) sem acessar o destino final.

Validação de Cache (304): O If-None-Match envia o token ETag da versão em cache do cliente. Se o servidor verificar que o recurso não mudou, ele retorna um status 304 Not Modified sem reenviar todo o corpo dos dados, economizando banda na rede.

3. Conclusão Técnica
Compreender a transmissão de bytes baseados em texto ASCII e o uso rigoroso das sequências de controle CRLF (\r\n) na camada de aplicação é fundamental para entender como os navegadores, clientes HTTP e servidores de alta performance trocam dados na web, permitindo depurar e desenvolver aplicações web robustas desde o nível do socket de rede.
