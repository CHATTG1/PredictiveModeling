---

copyright: years: 2016, 2017 lastupdated: "2017-11-16"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Bibliotecas do cliente

## Biblioteca do cliente da API comum <span class='tag--beta'>Beta</span>

Para usar o serviço IBM Watson Machine Learning com a linguagem de programação
Python, é possível usar PyPI para instalar a biblioteca do cliente
`watson-machine-learning-client`.

Para obter mais detalhes sobre como isso é feito, você pode verificar as anotações
de amostra
[Spark
MLlib](https://apsportal.ibm.com/analytics/notebooks/1fed143e-1877-42bd-b927-7d366e73745b/view?access_token=4b39718f9e1f1de55e6e67e8dcbb5f0cac848f390d73478d0dea9c1a8af24550) ou
[scikit-learn](https://dataplatform.ibm.com/analytics/notebooks/15b46bd5-dde2-4d59-9d7d-51cc0b860c8b/view?access_token=d8711ad6ae84b3a9c60d43966f961f66adc2c5b89fec18f24c85e40774080e9a)
que demonstram as seguintes técnicas:

* persistência de modelo
* implementação on-line
* pontuação usando o cliente da API comum

A documentação da biblioteca do cliente da API comum está disponível
[aqui](http://wml-api-pyclient.mybluemix.net/).

### Restrições

* O cliente da API comum está em **beta**.
* Para poder usar o cliente da API comum, deve-se instalar pelo menos um destes
pacotes: XGboost, scikit-learn ou pyspark.
* Apenas Python 3 é suportado.

## Biblioteca do cliente da API de repositório

Os wrappers da biblioteca do cliente para as APIs de REST do repositório do serviço
de aprendizado de máquina são desenvolvidos nas linguagens de programação Python e Scala.

Para obter mais informações sobre como usar as bibliotecas do cliente
`repository` para apresentar a persistência do modelo ao repositório
do Watson Machine Learning, consulte as
[anotações
de amostra](https://dataplatform.ibm.com/analytics/notebooks/89492fd6-a641-4819-9176-3d9381561df9/view?access_token=d80bef1a172d1d83d3721b101886337158457281774186f181a2e6a5b57f5ec7).

Para obter mais informações sobre as bibliotecas de repositório, consulte os tópicos a seguir:

* [Documentação do Scala Repository Module](https://watson-ml-staging-libs.mybluemix.net/repository-scala/)
* [Documentação do Python Repository Module](https://watson-ml-staging-libs.mybluemix.net/repository-python/)

### Restrições

As bibliotecas de repositório estão disponíveis apenas quando você usa o [IBM Data Science Experience](https://datascience.ibm.com).
