# FaceConv 🎭

> Conversor de imagens para avatar de login no SDDM (Display Manager)

Uma ferramenta simples e eficiente para definir avatares personalizados em ambientes Linux com SDDM (Simple Desktop Display Manager), diretamente a partir do gerenciador de arquivos.

## 📋 Sumário

- [Características](#-características)
- [Requisitos](#-requisitos)
- [Instalação](#-instalação)
- [Uso](#-uso)
- [Componentes](#-componentes)
- [Permissões](#-permissões)
- [Licença](#-licença)

## ✨ Características

- ✅ Conversão automática de imagens para avatar de login (256x256px)
- ✅ Integração com Nautilus (gerenciador de arquivos GNOME)
- ✅ Menu de contexto para fácil acesso
- ✅ Suporte a múltiplos formatos de imagem
- ✅ Redimensionamento inteligente com crop automático
- ✅ Validação de permissões
- ✅ Feedback visual com notificações
- ✅ Sem necessidade de executar como root

## 🔧 Requisitos

### Dependências Obrigatórias
- **ImageMagick** - Processamento de imagens
- **Nautilus (Arquivos)** - Gerenciador de arquivos GNOME
- **Python 3.x** - Extensão do Nautilus
- **SDDM** - Display Manager

### Dependências do Nautilus
- `nautilus-python` - Bindings Python para Nautilus
- `PyGObject` (gi) - Bindings para GObject/GTK
- `PyNotify` (Notify) - Sistema de notificações

### Sistema
- Linux (testado em distribuições baseadas em Arch)
- Acesso a `sudo` para ajustes de permissão

## 📦 Instalação

### Em Distribuições Arch Linux

```bash
# 1. Instale as dependências
sudo pacman -S imagemagick nautilus nautilus-python python-gobject

# 2. Clone o repositório
git clone https://github.com/elppans/faceconv.git
cd faceconv

# 3. Execute o instalador
sudo ./install.sh
```

### Instalação Manual

```bash
# 1. Copie o script principal
sudo cp usr/local/bin/faceconv /usr/local/bin/
sudo chmod +x /usr/local/bin/faceconv

# 2. Copie a extensão do Nautilus
sudo cp usr/share/nautilus-python/extensions/faceconv-menu.py \
     /usr/share/nautilus-python/extensions/

# 3. Reinicie o Nautilus
killall nautilus
```

## 🚀 Uso

### Via Nautilus (Método Recomendado)

1. Abra o **Nautilus** (Arquivos)
2. Navegue até uma imagem
3. Clique com botão direito na imagem
4. Selecione **"Definir como Avatar de Login"**
5. Confirme na janela de notificação

### Via Linha de Comando

```bash
faceconv /caminho/para/imagem.jpg
```

**Exemplos:**

```bash
# Usar uma imagem de perfil
faceconv ~/Pictures/meu-avatar.png

# Usar uma foto do diretório atual
faceconv ./foto.jpg
```

### O que acontece internamente

1. **Validação**: Verifica se o ImageMagick está instalado
2. **Redimensionamento**: Redimensiona a imagem para 256x256 pixels
3. **Crop**: Centraliza a imagem mantendo proporções
4. **Otimização**: Remove metadados e converte para PNG32
5. **Permissões**: Define permissões corretas (644)
6. **Ajuste de Diretório**: Configura permissões da home se necessário

## 📁 Componentes

### Script Principal: `usr/local/bin/faceconv`

Script Bash que realiza:
- Validação de permissões (não executa como root)
- Verificação de dependências (ImageMagick)
- Processamento da imagem
- Ajuste de permissões da home para SDDM

**Recursos:**
- Redimensionamento de 256x256px
- Gravity center para crop inteligente
- Conversão para PNG32 com transparência
- Remoção de metadados

### Extensão Nautilus: `usr/share/nautilus-python/extensions/faceconv-menu.py`

Integração Python com Nautilus que:
- Detecta cliques com botão direito em imagens
- Executa o script principal
- Exibe diálogos de sucesso/erro
- Fornece feedback ao usuário

**Funcionalidades:**
- Menu de contexto integrado
- Suporte a múltiplos formatos de imagem
- Tratamento de exceções
- Diálogos GTK personalizados

## 🔐 Permissões

O script gerencia automaticamente as permissões necessárias:

### Avatar (`~/.face.icon`)
```bash
chmod 644 ~/.face.icon
```

### Diretório Home
O SDDM precisa de permissão de execução (`x`) para listar o diretório home:

```bash
chmod a+x ~  # Adiciona execute para outros usuários
```

> **Nota**: O script pedirá sua senha uma vez para ajustar permissões da home, se necessário.

## 🐛 Solução de Problemas

### "Erro: 'imagemagick' não instalado"
```bash
# Arch Linux
sudo pacman -S imagemagick

# Debian/Ubuntu
sudo apt install imagemagick

# Fedora
sudo dnf install ImageMagick
```

### Extensão Nautilus não aparece
```bash
# Reinstale a extensão
sudo cp usr/share/nautilus-python/extensions/faceconv-menu.py \
     /usr/share/nautilus-python/extensions/

# Reinicie o Nautilus
killall nautilus
nautilus &
```

### Erro de permissão ao atualizar avatar
O script pedirá `sudo` automaticamente para ajustar permissões da home.

### Avatar não aparece no SDDM
Verifique as permissões:
```bash
ls -la ~/.face.icon
stat -c "%a" ~
```

## 📝 Estrutura do Projeto

```
faceconv/
├── README.md                                    # Este arquivo
├── LICENSE                                      # MIT License
├── usr/
│   ├── local/
│   │   └── bin/
│   │       └── faceconv                         # Script principal (Shell)
│   └── share/
│       └── nautilus-python/
│           └── extensions/
│               └── faceconv-menu.py             # Extensão Nautilus (Python)
└── install.sh                                   # Script de instalação
```

## 🔄 Fluxo de Funcionamento

```
Clique direito na imagem (Nautilus)
         ↓
Detecta tipo de arquivo (imagem)
         ↓
Mostra menu "Definir como Avatar de Login"
         ↓
Usuário clica no menu
         ↓
Executa /usr/local/bin/faceconv
         ↓
Valida permissões e dependências
         ↓
ImageMagick processa imagem
         ↓
Salva em ~/.face.icon
         ↓
Ajusta permissões (com sudo se necessário)
         ↓
Exibe notificação de sucesso/erro
```

## 💡 Dicas

### Padrão de Imagem Recomendado
- **Formato**: PNG ou JPG
- **Tamanho**: Mínimo 256x256px (maior é OK, será redimensionado)
- **Proporção**: Quadrada é ideal (evita distorção)
- **Conteúdo**: Foco no rosto/elemento principal centralizado

### Otimização
Imagens maiores serão automaticamente redimensionadas, mas é melhor começar com uma tamanho sensato:

```bash
# Pré-processar uma imagem grande
convert foto-grande.jpg -resize 512x512 foto-otimizada.jpg
faceconv foto-otimizada.jpg
```

## 📄 Licença

Este projeto está licenciado sob a **MIT License**.

```
Copyright (c) 2026 Elppans

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
```

Veja o arquivo [LICENSE](LICENSE) para detalhes completos.

## 🤝 Contribuindo

Contribuições são bem-vindas! Se encontrar bugs ou tiver sugestões:

1. Abra uma [Issue](https://github.com/elppans/faceconv/issues)
2. Descreva o problema ou sugestão
3. Inclua detalhes do seu ambiente

## 📞 Suporte

Para questões ou problemas:
- Abra uma [Issue no GitHub](https://github.com/elppans/faceconv/issues)
- Inclua seu sistema operacional e versão
- Descreva os passos para reproduzir o problema

## 🎉 Créditos

Desenvolvido por **Elppans** como uma ferramenta para facilitar o gerenciamento de avatares em ambientes Linux com SDDM.

---

**Versão**: 1.0  
**Última atualização**: 2026-07-26  
**Compatibilidade**: Linux com GNOME + SDDM
