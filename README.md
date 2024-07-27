Seja bem vindo ao repositório do presto 

# Tutorial: Ferramentas de Exploração e Pesquisa de Pulsar - PRESTO

## Introdução ao PRESTO

O PRESTO é uma suíte de software poderosa, desenvolvida por Scott Ransom, projetada para a pesquisa e análise de pulsares. Criado do zero e disponível sob a licença GPL (v2), o PRESTO é uma ferramenta essencial para astrônomos e pesquisadores interessados no estudo de pulsares, especialmente pulsares binários de milissegundos.

## Objetivo Principal

O principal objetivo do PRESTO é facilitar a detecção e análise de pulsares binários de milissegundos através de observações prolongadas de aglomerados globulares. No entanto, suas funcionalidades são versáteis e vão além desse escopo. O software também é amplamente utilizado em diversas pesquisas, incluindo a análise de integrações curtas e o processamento de grandes volumes de dados de raios-X.

## Características Técnicas

- **Linguagem de Programação**: O código-fonte do PRESTO é majoritariamente escrito em ANSI C, com várias rotinas mais recentes implementadas em Python.
- **Licença**: GPL (v2), o que garante que o software é livre para uso e modificação, respeitando os termos da licença.

## Aplicações do PRESTO

- **Detecção de Pulsar Binário de Milissegundos**: Ideal para observações prolongadas de aglomerados globulares, permitindo uma pesquisa mais eficiente desses pulsares.
- **Integrações Curtas**: Utilizado para a análise de dados em observações de integração curta.
- **Processamento de Dados de Raios-X**: Adequado para lidar com grandes volumes de dados provenientes de observações de raios-X.

## Processamento de Dados

Segundo Scott, o *PRESTO* foi desenvolvido com foco em portabilidade, facilidade de uso e eficiência de memória. Atualmente, ele é capaz de processar dados brutos de várias máquinas e formatos de pulsar, incluindo:

- Dados no formato de pesquisa *PSRFITS* (como os gerados pelo *GUPPI* no *GBT*, *PUPPI* e pelos Mock Spectrometers em Arecibo, além de muitos dados novos e arquivados do Parkes).
- Formatos de banco de filtros de 1, 2, 4, 8 e 32 bits (em ponto flutuante) da *SIGPROC*.
- Séries temporais compostas por dados em ponto flutuante de precisão única (ou seja, 4 bytes), acompanhados por um arquivo de texto `.inf` que os descreve.
- Tempos de chegada de fótons (ou eventos) em formatos ASCII ou binários de precisão dupla [GitHub PRESTO](https://github.com/presto).

No entanto, alguns formatos anteriormente suportados não estão mais incluídos:

- Processador Pulsar de Banda Larga Arecibo (WAPP) em Arecibo.
- Formatos de banco de filtros de 1 bit do Parkes e Jodrell Bank.
- SPIGOT no GBT.
- Máquina Pulsar Berkeley-Caltech (BCPM) no GBT.

Para processar esses formatos descontinuados, é recomendado verificar o ramo "clássico" do PRESTO, que não está mais em desenvolvimento ativo. Alternativamente, o *DSPSR* pode ser usado para converter esses formatos para o formato de banco de filtros `SIGPROC` ou, ainda melhor, para o formato de pesquisa *PSRFITS*. O *DSPSR* pode ser obtido em [DSPSR](https://github.com/DSPSR). Se você necessita de suporte para algum desses formatos no PRESTO moderno, entre em contato para que possamos avaliar a possibilidade de implementação [GitHub PRESTO](https://github.com/presto).

## Funcionalidades do Software

O PRESTO é composto por inúmeras rotinas projetadas para lidar com três áreas principais da análise de pulsares:

- **Preparação de Dados**: Detecção (`rfifind`) e remoção (`zapbirds`) de interferências, de-dispersão (`prepdata`, `prepsubband` e `mpiprepsubband`), barycentrização (via TEMPO).
- **Busca**: Busca de aceleração e jerk no domínio de Fourier (`accelsearch`), busca de pulsos únicos (`single_pulse_search.py`) e busca de modulação de fase ou bandas laterais (`search_bin`).
- **Dobragem**: Otimização de candidatos (`prepfold`) e geração de Tempo de Chegada (TOA) (`get_TOAs.py`).
- **Miscelânea**: Exploração de dados (`readfile`, `exploredat`, `explorefft`), planejamento de de-dispersão (`DDplan.py`), conversão de datas (`mjd2cal`, `cal2mjd`), bibliotecas Python para pulsares/astrofísica, criação de pulso médio, estimativa de densidade de fluxo, entre outros.
- **Ferramentas Pós-Busca de Pulsos Únicos**: Algoritmo de agrupamento (`rrattrap.py`), produção de gráficos diagnósticos de pulsos únicos (`make_spd.py`, `plot_spd.py` e `waterfaller.py`).

Além disso, são fornecidos muitos utilitários adicionais para várias tarefas frequentemente necessárias ao trabalhar com dados de pulsares, como conversões de tempo, transformadas de Fourier, exploração de séries temporais e FFT, inversão de bytes, entre outros [GitHub PRESTO](https://github.com/presto) [Fourier](https://github.com/fourier) [Jerk](https://github.com/jerk) [New](https://github.com/new).

## Instalação

Este tutorial é direcionado para a instalação do PRESTO no sistema operacional Linux. A instalação será realizada seguindo o processo padrão, conforme descrito no arquivo de referência do PRESTO.

O PRESTO tem alguns pré-requisitos e deve funcionar em um sistema semelhante ao Debian/Ubuntu. Portanto, certifique-se de instalar os seguintes pacotes:

- `FFTW3`
- `PGPLOT`
- `TEMPO`
- `GLIBv2`
- `CFITSIO`

Iniciaremos com a instalação principal do PRESTO e, posteriormente, abordaremos a instalação dos pacotes adicionais.


