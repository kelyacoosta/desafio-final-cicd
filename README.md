Desafio Final: CI/CD com GitHub Actions

Projeto da UC com uma esteira de CI/CD completa que valida, empacota e publica um site estático no GitHub Pages.

Site: https://kelyacoosta.github.io/desafio-final-cicd/
Release: https://github.com/kelyacoosta/desafio-final-cicd/releases/tag/v2.0.0
Estrutura do projeto
.
├── .github/workflows/ci.yml   # pipeline
├── index.html                 # página do site
├── style.css                  # estilos
└── README.md
Parte 1: A esteira completa

O workflow ci.yml roda a cada push na branch main e tem três jobs encadeados com needs:

Job	Função
Test	Roda o linter: HTMLHint no index.html e uma verificação simples no style.css
Build	Cria a pasta dist/, copia os arquivos do site e faz o upload do artefato
Deploy	Baixa o artefato e publica no GitHub Pages

Ordem de execução: Test → Build → Deploy. Se um job falha, os seguintes não rodam.

Parte 2: O erro proposital (Troubleshooting)

O que foi feito: no job Build, a pasta criada passou a se chamar build, mas o passo de upload do artefato continuou procurando dist/.

Resultado: a pipeline falhou no passo "Upload do artefato" do job Build, e o job Deploy nem chegou a rodar.

Linha do log que indica o problema:

Error: No files were found with the provided path: dist/. No artifacts will be uploaded.

Causa: desencontro de nomes. O Build criou build/, mas o upload procurava dist/, que não existia.

Correção: os passos "Criar pasta dist", "Copiar arquivos" e "Confirmar conteúdo" do job Build voltaram a usar dist, alinhados com o upload e o Deploy. Commit: fix: corrige caminho da pasta de artefato. Depois dele, os três jobs ficaram verdes.

Aprendizado: ao falhar, abrir o job que ficou vermelho, expandir o passo com erro e ler a linha que começa com Error:. O aviso DeprecationWarning: The punycode module is deprecated que aparece logo acima é só um alerta do Node e não tem relação com a falha.

Parte 3: O Selo de Ouro

Com o site no ar, foi criada a tag v2.0.0 e publicada a Release de conclusão da UC: v2.0.0 - Conclusão da UC.
