# Script JMeterr (Entidade Falecido)

Este documento descreve o funcionamento de dois trechos de código utilizados em testes no Apache JMeter.

---

## Trecho 1: Validação da variável `falecido`

```groovy
def falecido = vars.get("falecido_#") as int
log.info("\n ${falecido}")

if(falecido == 0) {
    AssertionResult.setFailure(true)
    AssertionResult.setFailureMessage("O registo pesquisado  nao esta falecido.")
}

def hasfalecido = vars.get("falecido_1")

log.info("Falecido: ${hasfalecido}")

if (hasfalecido == "N") {
    prev.setSuccessful(true) 
    prev.setResponseMessage("TESTE PASSOU: o registro não está falecido")
} else {
    log.error("TESTE FALHOU: falecido = ${hasfalecido}")
    prev.setSuccessful(false) 
    prev.setResponseMessage("Falecido diferente de N: ${hasfalecido}")
}
```

###  O que este cóodigo faz

### 1. Obtém o número de ocorrências

```groovy
def falecido = vars.get("falecido_#") as int
```

* Recupera a quantidade de valores encontrados para a variável `falecido`.
* O sufixo `_#` no JMeter indica **quantidade de matches** de um extractor (ex: Regex, JSON, etc).

---

### 2. Verifica se não houve resultados

```groovy
if(falecido == 0)
```

* Se nenhum registro foi encontrado:

  * Marca a asserção como falha
  * Define a mensagem de erro

---

### 3. Obtém o valor do primeiro resultado

```groovy
def hasfalecido = vars.get("falecido_1")
```

* Recupera o **primeiro valor encontrado**.

---

### 4. Valida o conteúdo do campo

```groovy
if (hasfalecido == "N")
```

* Se o valor for `"N"`:

  * Teste passa
  * Define mensagem de sucesso

* Caso contrário:

  * Teste falha
  * Registra erro no log
  * Retorna mensagem indicando valor inesperado

---

## Trecho 2: Log da resposta completa

```groovy
def response = prev.getResponseDataAsString()

log.info("ersponse completo: \n"+response)
```

### O que este código faz

### 1. Captura a resposta da requisição

```groovy
def response = prev.getResponseDataAsString()
```

* `prev` representa o **resultado da última requisição HTTP**
* Converte a resposta para **String**

---

### 2. Exibe no log

```groovy
log.info("ersponse completo: \n"+response)
```

* Mostra a resposta completa no log do JMeter
* Útil para:

  * Debug
  * Verificar conteúdo retornado pela API
  * Validar estrutura da resposta

---

## Resumo Geral

* O script:

  * Verifica se existe registro de "falecido"
  * Valida se o valor retornado é `"N"`
  * Marca o teste como sucesso ou falha
  * Exibe logs para análise


##  Conclusão

Este script é útil para validar regras de negócio em testes automatizados, garantindo que registros retornados estejam de acordo com o esperado (neste caso, não estarem marcados como falecidos).
