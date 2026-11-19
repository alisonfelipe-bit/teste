Markdown
# Tutorial Completo: PySide6 no GitHub Codespaces

## 1. Estrutura do Projeto
Mantenha a estrutura de pastas organizada na raiz do seu repositório:
* `app.py` (ou `app1.py`): Arquivo na raiz contendo o código Python da aplicação Qt.
* `.devcontainer/devcontainer.json`: Pasta oculta na raiz contendo a configuração do container.
* `.gitignore`: Adicione `self.pem` dentro deste arquivo para não enviar o certificado temporário para o GitHub.

---

## 2. Instalação de Dependências (Executar 1 vez ao criar o Codespace)
No terminal do Codespaces, instale o servidor gráfico virtual, o noVNC e todas as bibliotecas gráficas necessárias para o plugin `xcb` do Qt funcionar sem erros:

```bash
sudo apt-get update && sudo apt-get install -y \
  xvfb x11vnc fluxbox websockify novnc \
  libgl1 libegl1 libdbus-1-3 \
  libxkbcommon0 libxkbcommon-x11-0 \
  libxcb-cursor0 libxcb-icccm4 libxcb-image0 \
  libxcb-keysyms1 libxcb-randr0 libxcb-render-util0 \
  libxcb-shape0 libxcb-xfixes0 libxcb-xinerama0 \
  libfontconfig1 libxrender1
Em seguida, instale o PySide6 diretamente no Python do ambiente:

Bash
python -m pip install PySide6
3. Iniciar o Ambiente Gráfico Virtual (Sempre que abrir o Codespace)
Sempre que você iniciar ou reiniciar o Codespace, rode os comandos abaixo para ligar a tela virtual e a transmissão para o navegador:

Bash
# 1. Inicia o display virtual :99
Xvfb :99 -screen 0 1024x768x16 &
export DISPLAY=:99

# 2. Inicia o gerenciador de janelas e o servidor VNC
fluxbox &
x11vnc -display :99 -forever -shared -rfbport 5900 &

# 3. Gera o certificado SSL local e inicia o noVNC na porta 6080
openssl req -new -x509 -days 365 -nodes -out self.pem -keyout self.pem -subj "/CN=localhost"
websockify --web=/usr/share/novnc/ --cert=self.pem 6080 localhost:5900 &
4. Abrir a Tela no Navegador e Rodar o Código
Vá para a aba Portas (ao lado de "Terminal").

Localize a porta 6080, passe o mouse sobre o link em Endereço Encaminhado e clique no ícone do globo (Abrir no Navegador).

Se aparecer um aviso de segurança no navegador, clique em Avançado > Ir para o site.

Clique em vnc.html e depois no botão Conectar (Connect). A tela preta com o logo do Ubuntu/Fluxbox será exibida.

Volte ao terminal do Codespaces e execute o seu arquivo Python:

Bash
export DISPLAY=:99
python app1.py
Volte para a aba do navegador (noVNC) para interagir com a janela do seu aplicativo.

5. Solução de Problemas (Reiniciar tudo se travar)
Caso a tela trave ou alguma porta fique presa, rode este comando para limpar os processos antigos e reiniciar:

Bash
killall Xvfb x11vnc websockify fluxbox 2>/dev/null
Depois, basta repetir os comandos do Passo 3.


### Como substituir no GitHub:
1. No GitHub (na mesma tela da imagem), clique no **ícone do lápis** (canto superior direito do quadro do `README.md`)[cite: 12].
2. Selecione tudo que está lá (`Ctrl + A`), apague (`Backspace`) e cole (`Ctrl + V`) o texto copiado acima.
3. Clique no botão verde **Commit changes...** para salvar. Assim ele ficará sem nenhuma mar

Se quiser, posso montar o conteúdo pronto do arquivo .devcontainer/devcontainer.json para que toda a parte do Passo 2 seja instalada automaticamente sempre que você criar um novo Codespace. Deseja que eu gere esse arquivo?
