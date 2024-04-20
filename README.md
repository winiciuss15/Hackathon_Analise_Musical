# Hackathon Analise Musical
Esse projeto foi desenvolvido par a o desafio de Hackathon da Pós Graduação em Gestão e Ciência de dados da Universidade Federal de Mato Grosso - UFMT.  

## Sobre

Os dados são coletados da [página da SEFAZ do Registro e Controle da Renúncia](https://www.sefaz.mt.gov.br/rcr-fe/consultacredenciados). A página corresponde ao sistema de cadastramento, registro e acompanhamento eletrônico dos pedidos de fruição de benefícios fiscais, conforme informa a [portaria 200/2019 da SEFAZ-MT](https://app1.sefaz.mt.gov.br/0325677500623408/7C7B6A9347C50F55032569140065EBBF/46636B2ED3F66CA0042584D600496AB5#:~:text=Art.,de%20frui%C3%A7%C3%A3o%20de%20benef%C3%ADcios%20fiscais.) . A API (https://www.sefaz.mt.gov.br/rcr-fe-api/v1/processo/consultacredenciados) possui como parâmetros a página correspondente ao site, com a respectiva quantidade de itens a serem renderizados, da seguinte forma: /consultacredenciados?page=**pagina**&size=**size**, onde **pagina** e **size** são números inteiros maiores que 0.

O projeto assume python >=3.10 e <3.12, a biblioteca [poetry](https://python-poetry.org/) para gerenciamento das dependências e ambiente. O projeto também assume a instalação de docker e docker compose.

A imagem do airflow utilizada pode ser conferida através da [página de docker-compose do airflow](https://airflow.apache.org/docs/apache-airflow/stable/howto/docker-compose/index.html).

## Estrutura

Dentro da pasta [DAGs](/prodeic/airflow/dags/), estão todos os arquivos responsáveis por uma etapa do pipeline com a base PRODEIC (por exemplo, coleta e carga) e os códigos SQL para a criação das tabelas no banco de dados.
 
A pasta [utils](/prodeic/airflow/utils/) contém um arquivo exemplo da estrutura dos requisitos que definem a extração dos dados, outros arquivos auxiliares futuros necessários serão inclusos nela. A pasta está como volume para dentro do container, dentro de /opt/airflow/utils, para acesso aos arquivos que são baixados da API da SEFAZ-MT e depois excluídos.

Lembre-se de na raiz do diretório criar uma pasta **.vscode**, com um arquivo **settings.json** com as configurações do projeto. Algumas configurações abaixo que são úteis para o desenvolvimento:
```json
{
    "pylint.path": [
        "pylint bin_path"
    ],
}
```

Lembrando que para obter o diretório correspondente ao pylint, que você pode obter através do comando abaixo, em que, por exemplo, você pegará o pylint da informação **Executable** de **virtualenv**:
```bash
poetry env info
```

Exemplo de saída, coloque na cofiguração de _pylint.path_, substitua **python** por **pylint**, podendo ignorar a extensão .exe

## Build

O projeto tem os seguintes requisitos:

- [Pyenv](https://github.com/pyenv/pyenv) - Gerenciamento de versões do python
- [Poetry](https://python-poetry.org/) - Gerenciamento de depedências do python
- [Airflow](https://airflow.apache.org/) - Scheduler para execução das pipelines
- [Docker](https://www.docker.com/) - Containerização do projeto, facilitar o deploy

Instalação do python 3.11.5 e determinação do python para este repositório
```bash
pyenv install 3.11.5
pyenv local 3.11.5
```

Configuração do ambiente com o poetry e instalação das dependências, o argumento **--no-root** pois não será utilizado esse projeto como um pacote.
```bash
poetry env use 3.11.5
poetry shell
poetry install --no-root
```

Instalação, usando o docker, você deve estar na pasta raiz devido ao arquivo requirements.txt, lembrando que o mesmo deve ser atualizado sempre que novas dependências forem adicionadas ou removidas:
```bash
poetry export -o requirements.txt
docker build . -f airflow/Dockerfile --pull --tag observatoriofiemt/sousapedroso-prodeic-test:0.0.1 --no-cache
```

Depois de construir a imagem, rode com o docker compose dentro do diretório airflow:
```bash
docker compose up
```

## Contribuidores<br>

<table>
  <tr>
    <td align="center">
      <a href="https://github.com/thiago-vbarbosa">
        <img src="https://avatars1.githubusercontent.com/u/78553616" width="100px;" alt="Foto do Winicius Sabino"/><br>
        <sub>
          <b>Alexandre</b>
        </sub>
      </a>
    </td>
    <td align="center">
      <a href="https://github.com/winiciussabino">
        <img src="https://avatars1.githubusercontent.com/u/78553616" width="100px;" alt="Foto do Winicius Sabino"/><br>
        <sub>
          <b>Alberto</b>
        </sub>
      </a>
    </td>
    <td align="center">
      <a href="https://github.com/winiciussabino">
        <img src="https://avatars1.githubusercontent.com/u/78553616" width="100px;" alt="Foto do Winicius Sabino"/><br>
        <sub>
          <b>Winicius Sabino</b>
        </sub>
      </a>
    </td>
     <td align="center">
      <a href="https://github.com/winiciussabino">
        <img src="https://avatars.githubusercontent.com/u/37580243?v=4" width="100px;" alt="Foto do Winicius Sabino"/><br>
        <sub>
          <b>Winicius Sabino</b>
        </sub>
      </a>
    </td>
  </tr>
</table>
