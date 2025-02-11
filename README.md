# Subir de forma manual GCR na GCP
Explicação de como subir um GCR de forma manual

# Passo a Passo: Migração para o Cloud Run (GCP)

## 1. Dockerizar a Aplicação
- Crie um `Dockerfile` para a aplicação.
- Construa a imagem localmente com:
  ```sh
  docker build -t meurepo/minha-imagem-docker .
  ```
- Envie a imagem para um repositório público ou privado, como o Docker Hub (opcional):
  ```sh
  docker push meurepo/minha-imagem-docker
  ```

## 2. Enviar a Imagem para o Container Registry da GCP
Após a etapa anterior, siga os passos abaixo dentro do **Cloud Shell** da GCP:

### 2.1 Baixar a Imagem do Docker Hub (se necessário)
```sh
docker pull docker.io/meurepo/minha-imagem-docker
```

### 2.2 Autenticar no Container Registry
Certifique-se de estar autenticado no Google Cloud:
```sh
gcloud auth configure-docker us.gcr.io
```

### 2.3 Verificar se a Imagem foi Baixada
```sh
docker images
```
Isso listará as imagens disponíveis localmente.

### 2.4 Taguear a Imagem para o Container Registry
Copie o **ID da imagem** listado pelo comando acima e execute:
```sh
docker tag <ID_DA_IMAGEM> us.gcr.io/<ID_DO_PROJETO>/meu-projeto-gcr
```
Exemplo prático:
```sh
docker tag 111aaa222 us.gcr.io/key-exemplochave-111222333/meu-projeto-gcr
```

### 2.5 Enviar a Imagem para o Container Registry
```sh
docker push us.gcr.io/<ID_DO_PROJETO>/meu-projeto-gcr
```
Exemplo:
```sh
docker push us.gcr.io/key-exemplochave-111222333/meu-projeto-gcr
```

---

## 3. Implantar no Cloud Run

1. **Acesse o Cloud Run na Console da GCP**
2. Clique em **"Create Service"**
3. Escolha a **região** e defina um **nome** para o serviço
4. Clique em **"Next"**
5. Em **"Container URL"**, selecione a imagem enviada para o Container Registry
   - Clique em **"Select"** para exibir as imagens disponíveis
   - Escolha a imagem correta
6. Clique em **"Advanced Settings"** e configure a **porta do container** se necessário
7. Clique em **"Next"** e depois em **"Create"**

---

## Observações
✔ O **Container Registry** pode ser substituído pelo **Artifact Registry**, pois o primeiro está sendo descontinuado. Para usar o **Artifact Registry**, basta modificar a URL para:
   ```sh
   docker tag <ID_DA_IMAGEM> us-docker.pkg.dev/<ID_DO_PROJETO>/meu-repositorio-gcr/meu-projeto-gcr
   docker push us-docker.pkg.dev/<ID_DO_PROJETO>/meu-repositorio-gcr/meu-projeto-gcr
   ```
