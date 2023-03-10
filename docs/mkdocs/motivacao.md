# A História da Criação da Biblioteca

## Desafios de Aprendizado

A dinâmica de processamento de grandes volumes de dados é, sem dúvidas, uma vertente altamente desafiadora no mundo analítico. Em meio às inúmeras ferramentas e *frameworks* que se fazem presentes hoje em dia no mercado, o [Spark](https://spark.apache.org/) possui um destaque especial por, entre outros fatores, entregar uma gigantesca gama de possibilidades em termos de desenvolvimento de soluções escaláveis.

???+ tip "Frameworks e serviços de Analytics em provedores cloud"
    Atualmente, o estado da arte em termos de desenvolvimento de aplicações resilientes e escaláveis gira em torno do uso de provedores *cloud* capazes de entregar um gigantesco leque de serviços que tangem todas as etapas do processo de geração de valor. No ramo de *analytics*, serviços AWS como o [AWS Glue](https://aws.amazon.com/glue/) e o [Amazon EMR](https://aws.amazon.com/emr/) possuem grande aplicabilidade e utilizam, em seu interior, o *framework* Spark como principal motor de processamento de grandes volumes de dados.

:material-alert-decagram:{ .mdx-pulse .warning } Por maiores que sejam as facilidades entregues por estes e outros *frameworks* e serviços analíticos, ainda existe um fator de aprendizado que exige de seus usuários entender alguns detalhes técnicos importantes antes de mergulhar à fundo e extrair todo o potencial entregue por tais ferramentas.

Em alguns casos, a necessidade rápida em codificar transformações e obter resultados passa por cima do entendimento de conceitos teóricos fundamentais para uma compreensão holística do processo. Afinal, nem todos sabem a fundo o significado de termos como, por exemplo, `SparkSession`, `SparkContext`, `GlueContext`, `DynamicFrame` existentes quando se quer implantar um *job* Glue na AWS.

???+ quote "E assim surgiu o questionamento:"
    
    "*Como entregar ao usuário uma forma rápida, fácil e de baixa complexidade para que o mesmo possa desenvolver e codificar jobs que usam o Spark como *background* na AWS sem se preocupar com definições 'burocráticas' e focando apenas na aplicação das regras de transformações de dados?*".

## Proposta de Padronização de Jobs

Diante do cenário acima exemplificado, o *sparksnake* nasce como uma tentativa de proporcionar aos usuários uma forma de padronizar seus *jobs* criados em serviços como Glue e EMR na AWS através das melhores práticas de código, funcionalidades prontas e outras inúmeras vantagens que giram em torno da simplificação e eficiência de código.

Para que se tenha uma ideia sobre a aplicação prática da proposta da biblioteca *sparksnake*, imagine o seguinte bloco de código abaixo comumente encontrado em *boilerplates* de iniciação de *jobs* Glue na AWS:

???+ example "Boilerplate padrão de um job Glue na AWS"

    ```python
    import sys
    from awsglue.transforms import *
    from awsglue.utils import getResolvedOptions
    from pyspark.context import SparkContext
    from awsglue.context import GlueContext
    from awsglue.job import Job

    ## @params: [JOB_NAME]
    args = getResolvedOptions(sys.argv, ['JOB_NAME'])

    sc = SparkContext()
    glueContext = GlueContext(sc)
    spark = glueContext.spark_session
    job = Job(glueContext)
    job.init(args['JOB_NAME'], args)
    ```

    ??? question "O que é tudo isso?"

        No código acima, é possível visualizar uma série de classes diferentes (`awsglue`) e objetos jamais vistos em outros cenários externos ao Glue.

        - O que essa função `getResolverOptions` está fazendo?
        - O que é um `GlueContext`?
        - O que é essa classe do tipo `Job` que recebe o `GlueContext`?
    
Considerando usuários com pouco ou nenhum contato com o serviço Glue, este *boilerplate*, mesmo com toda sua simplicidade, pode assustar um pouco.

Por outro lado, usuários avançados que conhecem cada detalhe destas classes e objetos do Glue devem se questionar: *"Todas as vezes que eu inicio um job Glue na AWS devo definir esse bloco inicial de código. Seria possível simplificá-lo através da chamada de uma única função?"*

:star:{ .heart } E aqui já é possível ver a proposta do *sparksnake* se materializando. Para este simples exemplo, existe uma funcionalidade da biblioteca que permite obter todos os insumos de contexto Glue e sessão Spark além da inicialização do job Glue com apenas **uma única linha de código!**

???+ example "Inicializando jobs Glue na AWS com sparksnake"

    ```python
    # Importando bibliotecas
    from sparksnake.manager import SparkETLManager

    # Inicializando objeto da classe e job Glue
    spark_manager = SparkETLManager(mode="glue", argv_list=["JOB_NAME"], data_dict={})
    spark_manager.init_job()
    ```

Fácil, não é mesmo? Esta é apenas uma das inúmeras funcionalidades criadas especialmente para facilitar a jornada de desenvolvimento de aplicações Spark integradas (ou não) ao Glue. Não deixe de navegar pela documentação para absorver todo este mar de possibilidades e aprimorar, de uma vez por toda, o processo de criação de jobs!