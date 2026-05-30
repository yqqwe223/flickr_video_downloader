# Flickr Video Downloader 📥

> Ferramenta web simples para guardar vídeos públicos do Flickr no teu dispositivo. Sem complicações, sem rastreamento, funciona na mesma.

## 👋 Porque é que isto existe?

Sejamos sinceros: às vezes estás a navegar no Flickr e encontras um vídeo que realmente precisa de ficar guardado — um tutorial de fotografia, um clip de bastidores, ou até algo que tu próprio carregaste e agora queres fazer backup. Mas descarregar isso de forma simples? Nem sempre é direto.

Foi por isso que escrevi esta ferramenta, primeiro para uso pessoal, e depois pensei: "Porque não partilhar?". Sem funcionalidades desnecessárias, sem tracking de utilizadores, sem obrigar a criar conta. Basta colar um link público do Flickr, clicar em "Analisar", e se o vídeo estiver acessível, aparecem as opções para descarregar. Só isso.

Todo o processamento acontece no servidor: não registo o que descarregas, não guardo históricos, não recolho dados pessoais. A tua privacidade fica contigo.

## ✨ O que realmente faz

- **Suporta links comuns do Flickr**: Funciona com álbuns públicos, páginas de vídeo de utilizadores e links partilhados diretos — desde que não sejam privados ou protegidos por password
- **Mostra qualidades disponíveis**: Quando o Flickr oferece várias resoluções (Original, Alta, Standard), podes escolher qual queres descarregar
- **Não precisa de login**: Apenas processa conteúdo acessível publicamente; nunca pede as tuas credenciais do Flickr
- **Interface limpa e responsiva**: Funciona bem em telemóvel, tablet ou desktop sem frameworks frontend pesados
- **Proteção básica contra abusos**: Limita automaticamente requests por IP para evitar sobrecarga e manter o serviço estável para todos
- **Processamento não bloqueante**: Mesmo ao analisar vídeos longos, o teu separador do browser não congela, a experiência mantém-se fluida

## 🛠 O que está por baixo do capô

| Camada | Tecnologias |
|--------|-------------|
| Backend | Python 3.11, Django 4.2 LTS |
| Parsing | httpx, lxml, regex para extração de metadata |
| Frontend | HTML5 semântico, CSS3 leve, JavaScript vanilla |
| Deploy | Gunicorn + Nginx, compatível com Docker |
| Utilities | python-dotenv, django-ratelimit, whitenoise |

Zero bibliotecas de IA. Zero chamadas API externas que "telefonem para casa". Apenas requests HTTP standard e parsing HTML escrito com cuidado — código que consegues ler, perceber e modificar sem dores de cabeça.

## 🚀 Como correr na tua máquina

### O que precisas
- Python 3.10 ou superior
- pip + venv (ou virtualenv)
- Conhecimento básico da estrutura de projetos Django

### Configuração para desenvolvimento

```bash
# Clonar o repositório
git clone https://github.com/o-teu-username/flickr-downloader-po.git
cd flickr-downloader-po

# Criar e ativar ambiente virtual
python -m venv .venv
source .venv/bin/activate  # Linux/macOS
# ou .venv\Scripts\activate  # Windows

# Instalar dependências
pip install -r requirements.txt

# Configurar variáveis de ambiente
cp .env.example .env
# Editar .env com os teus valores (SECRET_KEY, DEBUG, etc.)

# Correr migrations e iniciar servidor de desenvolvimento
python manage.py migrate
python manage.py runserver
```

Depois abre `http://127.0.0.1:8000` no browser.

### Notas para produção

- Definir `DEBUG=False` e configurar corretamente `ALLOWED_HOSTS` com os teus domínios
- Correr atrás de Nginx usando Gunicorn (ou uWSGI se preferires)
- Ativar HTTPS ao nível do proxy
- Recolher ficheiros estáticos: `python manage.py collectstatic`
- Se o tráfego aumentar, considera adicionar Redis para cache

Exemplo de comando Gunicorn:
```bash
gunicorn config.wsgi:application \
  --bind 127.0.0.1:8000 \
  --workers 2 \
  --timeout 90
```

Se aumentares o número de workers, fica de olho no uso de memória — o parsing de vídeo pode ser um pouco pesado.

## 📋 Como usar

1. Encontra uma página pública do Flickr com vídeo (álbum, perfil de utilizador ou link partilhado)
2. Copia o URL e cola no campo de input da ferramenta
3. Clica em "Analisar" — o backend vai extrair os streams de vídeo disponíveis
4. Se resultar, aparecem botões de download com etiquetas de resolução
5. Seleciona a opção que preferes; o ficheiro descarrega através do teu browser

> Nota: Apenas vídeos acessíveis publicamente funcionam. Álbuns privados, conteúdo só para amigos, vídeos protegidos por password ou com restrições regionais vão devolver erro. Isto é intencional — a ferramenta respeita as definições de privacidade do Flickr.

## ⚠️ Por favor, lê isto

Esta ferramenta foi desenvolvida **exclusivamente para uso pessoal e não comercial**. Exemplos de uso adequado:
- Fazer backup de vídeos que tu próprio carregaste no Flickr
- Guardar conteúdo educativo ou de referência partilhado publicamente para estudo offline
- Pesquisa ou propósitos de acessibilidade dentro das diretrizes de fair use

**Tu és responsável por**:
- Cumprir os [Termos de Serviço do Flickr](https://www.flickr.com/help/terms)
- Respeitar os direitos de autor e licenças Creative Commons dos criadores
- Seguir a legislação aplicável na tua jurisdição sobre cópia de conteúdo digital

Não monitorizo downloads e não assumo responsabilidade por uso indevido. Por favor, não uses esta ferramenta para:
- Scraping em massa ou recolha automatizada de conteúdo
- Redistribuir material protegido por direitos de autor sem permissão explícita
- Contornar definições de privacidade ou controlos de acesso
- Serviços comerciais ou re-hosting sem autorização prévia

Se tens dúvidas se o teu caso de uso é apropriado, provavelmente não é. Na dúvida, consulta primeiro o criador do conteúdo.

## 🤝 Queres ajudar?

Encontraste um bug? Achas que o parsing podia ser mais robusto? Tens uma ideia para melhorar a interface? Contribuições são bem-vindas — sem barreiras.

### Como contribuir
1. Faz fork do repo e cria uma branch para a tua funcionalidade (`git checkout -b fix/interface-mobile`)
2. Faz alterações em commits pequenos e lógicos, com mensagens claras
3. Testa localmente — garante que as funcionalidades existentes continuam operacionais
4. Abre um Pull Request com uma descrição concisa do que mudou e porquê

### Estilo de código
- Backend: Segue PEP 8, adiciona type hints onde melhorem a legibilidade
- Frontend: Mantém JavaScript no mínimo; prioriza progressive enhancement em vez de frameworks pesados
- Commits: Usa prefixos convencionais (`feat:`, `fix:`, `docs:`, `chore:`, etc.)

### Reportar problemas
Ao abrir um bug, por favor inclui:
- O URL do Flickr afetado (se for partilhável)
- Nome + versão do browser, sistema operativo, tipo de dispositivo
- Passos para reproduzir o problema
- Comportamento esperado vs. comportamento observado

Screenshots ou logs da consola também ajudam, especialmente para problemas frontend.

## 🔧 Opções de configuração

| Variável | Propósito | Exemplo |
|----------|-----------|---------|
| `DEBUG` | Ativa/desativa modo debug do Django | `False` |
| `SECRET_KEY` | Chave de segurança do Django | `a-tua-string-aleatoria-segura` |
| `MAX_VIDEO_SIZE_MB` | Rejeita ficheiros maiores que X MB | `500` |
| `RATE_LIMIT_PER_MIN` | Máximo de requests por IP por minuto | `10` |
| `ALLOWED_HOSTS` | Domínios permitidos (separados por vírgula) | `.oteudominio.pt` |

Todas as configurações são carregadas via `python-dotenv`; nenhum dado sensível está hardcoded no código fonte. Em produção, roda a tua `SECRET_KEY` periodicamente.

## 📄 Licença

Licença MIT — consulta o ficheiro [LICENSE](./LICENSE) para o texto completo.  
Podes usar, modificar e distribuir este software livremente, desde que conserves o aviso de copyright original.

## 📬 Contacto e suporte

- Reportes de bugs e sugestões de funcionalidades: usa o separador Issues do GitHub
- Perguntas gerais: support@twittervideodownloaderx.com
- Vulnerabilidades de segurança: por favor, notifica por email antes de qualquer divulgação pública

Tento responder a issues em poucos dias. Se passou mais tempo e não obtiveste resposta, não hesites em voltar a escrever — às vezes as coisas escapam.

---

*Este projeto não está afiliado, endossado nem relacionado com Flickr / SmugMug, Inc. Todas as marcas registadas, logótipos e direitos de conteúdo pertencem aos respetivos proprietários.*

*Última atualização: maio  | Versão 1.2.0*

*Demo ao vivo: https://twittervideodownloaderx.com/flickr_downloader_po*

*Escrito por uma pessoa, para pessoas. Nenhuma inteligência artificial participou na redação deste README ou do código do projeto.*