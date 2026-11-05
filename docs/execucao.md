# Instruções de execução

Requisito: JDK 17 ou superior.

## Compilação

Execute na raiz do projeto:

```bash
javac -d out src/servidor/*.java src/cliente/*.java
```

## Servidor

```bash
java -cp out servidor.Servidor
```

## Cliente

```bash
java -cp out cliente.Cliente
```

Nesta etapa, os programas exibem apenas mensagens iniciais. A comunicação TCP será implementada depois.
