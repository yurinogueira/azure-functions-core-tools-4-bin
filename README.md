# azure-functions-core-tools-4-bin

Pacote AUR (Arch User Repository) com os binários pré-compilados da **versão 4** do **Azure Functions Core Tools** (`func`), alinhado com a [documentação oficial da Microsoft para Linux](https://learn.microsoft.com/en-us/azure/azure-functions/functions-run-local).

## Por que `azure-functions-core-tools-4-bin`?

Na documentação oficial da Microsoft e nos repositórios oficiais para distribuições Linux (Debian/Ubuntu/RHEL), o pacote da versão 4 é distribuído sob o nome `azure-functions-core-tools-4` e instalado no diretório versionado `/usr/lib/azure-functions-core-tools-4`.

Este pacote foi criado para seguir a convenção oficial da Microsoft no Arch Linux e corrigir diversos problemas históricos presentes no pacote `azure-functions-core-tools-bin`:

1. **Caminho de Instalação Oficial (`/usr/lib/azure-functions-core-tools-4/func`):**
   - Ferramentas do ecossistema .NET (como o *Project Tye* via `FuncFinder.cs`, extensões de IDE e scripts standalone) buscam especificamente por `/usr/lib/azure-functions-core-tools-4/func`.
   - Inclui também um symlink de retrocompatibilidade em `/usr/lib/azure-functions-core-tools` apontando para `/usr/lib/azure-functions-core-tools-4`.
2. **Permissões de Execução Corrigidas (`chmod 755`):**
   - Os arquivos `.zip` de release não preservam bits de permissão Unix. Além do binário principal `func`, este pacote garante permissão de execução para `gozip` (evitando erros de `Permission denied` durante o `func azure functionapp publish`) e para os binários de host in-process (`in-proc6/func` e `in-proc8/func`).
3. **Pacote Limpo (Sem Vazamento do `.zip`):**
   - Evita que o symlink do próprio arquivo `Azure.Functions.Cli.linux-*.zip` de `$srcdir` seja copiado para dentro de `/usr/lib/` no pacote final.
4. **Preservação de Binários Self-Contained (`!strip`):**
   - Define `options=('!strip' 'staticlibs')` para impedir que o `strip` do `makepkg` corrompa executáveis e assemblies self-contained do .NET.
5. **Suporte Multi-Arquitetura (`x86_64` e `aarch64`) e Dependências Nativas:**
   - Suporta tanto `x86_64` quanto `aarch64` (`linux-arm64`) e declara explicitamente as bibliotecas nativas necessárias (`glibc`, `gcc-libs`, `icu`, `openssl`, `zlib`).

## Instalação

### Manualmente via `makepkg`

```bash
git clone https://github.com/YuriNogueira/azure-functions-core-tools-4-bin.git
cd azure-functions-core-tools-4-bin
makepkg -si
```

### Verificando a instalação

```bash
func --version
```
