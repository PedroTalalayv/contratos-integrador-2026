# Imagem do front do módulo, servida sob /modulos/contratos/ (Contrato §12.8).
FROM node:22-alpine AS build
WORKDIR /src
COPY package.json package-lock.json ./
RUN npm ci
COPY . .
# Site key pública do reCAPTCHA (VITE_* é gravado no build). O .env é excluído do contexto,
# então o valor padrão precisa estar aqui. Não é segredo: já consta em front/.env.example.
ARG VITE_RECAPTCHA_SITE_KEY=6LdBrxsuAAAAAAN39QoEEgs7_Q2jpXT6WjJB-ogo
ENV VITE_RECAPTCHA_SITE_KEY=$VITE_RECAPTCHA_SITE_KEY
RUN npm run build

FROM nginx:1.27-alpine
ENV PORTA=3004
COPY nginx/default.conf.template /etc/nginx/templates/default.conf.template
COPY --from=build /src/dist /usr/share/nginx/html/modulos/contratos
EXPOSE 3004
