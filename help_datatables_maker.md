# Documentação Técnica: WebrunDataTable

O **WebrunDataTable** é um componente de grade de dados avançado para o ambiente Webrun/Maker, oferecendo alta performance, temas customizáveis e uma API programática completa.

---

## 🏗️ Inicialização e Opções Globais

```javascript
new WebrunDataTable(containerId, options);
```

### Configurações do Objeto `options`

| Propriedade | Tipo | Padrão | Descrição |
| :--- | :--- | :--- | :--- |
| `theme` | `String` | `'default'` | Tema visual (`modern`, `project`, `grid`). Carrega o CSS automaticamente. |
| `datasetSource`| `String` | `'client'` | Origem dos dados: `'client'` (JSON local) ou `'server'` (SQL via Proxy). |
| `sqlQuery` | `String` | `''` | Consulta SQL (usada se `datasetSource: 'server'`). |
| `dataJSON` | `String` | `'[]'` | JSON de dados inicial (usada se `datasetSource: 'client'`). Veja exemplo abaixo. |
| `locale` | `String` | `'en_us'` | Localização da UI (ex: `'pt_BR'`). |
| `clickableRows` | `Boolean` | `false` | Habilita cursor de clique e evento `onRowClick`. |
| `disableScrollY`| `Boolean` | `false` | Se `true`, a tabela expande em altura e remove a rolagem interna do corpo. |
| `rowHeight` | `Number` | `null` | Altura fixa das linhas (células) em pixels. |
| `headerHeight` | `Number` | `null` | Altura fixa do cabeçalho em pixels. |
| `verticalAlignment` | `String`| `'top'` | Alinhamento vertical das células (`top`, `middle`, `bottom`). |
| `radius` | `Number` | `0` | Raio das bordas arredondadas (cantos superiores da tabela/cabeçalho). |
| `filterCompact` | `Boolean` | `false` | Se `true`, remove labels dos filtros e usa placeholders para economizar espaço. |
| `headerBackground`| `String` | `null` | Cor de fundo do cabeçalho (Hex ou Nome). |
| `headerColor` | `String` | `null` | Cor do texto do cabeçalho. |
| `borderColor` | `String` | `null` | Cor das bordas da tabela. |
| `rowColor` | `String` | `null` | Cor do texto das linhas. |
| `rowBackground` | `String` | `null` | Cor de fundo das linhas. |
| `rowStripedBackground`| `String` | `null` | Cor de fundo das linhas alternadas (se `striped: true`). |
| `showHorizontalBorders`| `Boolean` | `null` | Controla visibilidade das bordas horizontais (`true`/`false`). |
| `showVerticalBorders`| `Boolean` | `null` | Controla visibilidade das bordas verticais (`true`/`false`). |
| `showOuterBorder` | `Boolean` | `null` | Controla visibilidade da borda externa da tabela. |
| `showSearch` | `Boolean` | `true` | Exibe ou oculta o campo de busca global. |
| `showFilter` | `Boolean` | `true` | Exibe ou oculta a barra de filtros personalizados. |
| `showLength` | `Boolean` | `true` | Exibe ou oculta o seletor de registros por página. |
| `showInfo` | `Boolean` | `true` | Exibe ou oculta o rodapé informativo ("Mostrando 1 de 10..."). |
| `showPagination` | `Boolean` | `true` | Exibe ou oculta os controles de paginação. |
| `showOrdering` | `Boolean` | `true` | Habilita ou desabilita ordenação por colunas. || `showHorizontalBorders` | `Boolean` | `false` | Se `true`, exibe bordas horizontais. |
| `showVerticalBorders` | `Boolean` | `false` | Se `true`, exibe bordas verticais. |
| `showOuterBorder` | `Boolean` | `false` | Se `true`, exibe borda externa. |
| `showLength` | `Boolean` | `false` | Se `true`, exibe o comprimento da tabela. |
| `radius` | `Number` | `5` | Raio das bordas arredondadas. |
| `showSearch` | `Boolean` | `false` | Se `true`, exibe o campo de busca. |
| `showLength` | `Boolean` | `false` | Se `true`, exibe o comprimento da tabela. |
| `showLength` | `Boolean` | `false` | Se `true`, exibe o comprimento da tabela. |
---

## 📋 Configuração das Colunas (`columns`)

A propriedade `columns` define o comportamento e a aparência de cada campo.

### Propriedades Base
- **`column`**: Chave do campo no JSON.
- **`label`**: Título no cabeçalho.
- **`align`**: Alinhamento horizontal (`left`, `center`, `right`).
- **`size`**: Largura da coluna (ex: `'100px'`, `'15%'`).
- **`show`**: Visibilidade inicial (`true`/`false`).
- **`render`**: Template HTML flexível usando `${campo}`.

### Tipos de Colunas (`type`)
1. **`numeric`**: Suporta `format` (ex: `'R$ %.2f'`).
2. **`date`**: Suporta `format` de entrada/saída (`'dmy'`, `'mdy'`, `'ymd'`).
3. **`image`**: Renderiza URL. Suporta `format: 'circle'` para avatares.
4. **`checkbox`**: Coluna de seleção de linha (geralmente ligada ao ID).
5. **`button`**: Botão de ação único. Requer `name`, `buttonType`, `buttonIcon` e `action`.
6. **`group-button`**: Múltiplos botões. Propriedades aceitam Arrays de strings.

### 🔘 Detalhamento de Botões (`button` e `group-button`)

Para colunas de ação, é necessário configurar as propriedades visuais e comportamentais.

| Propriedade | Descrição | Exemplo (`button`) | Exemplo (`group-button`) |
| :--- | :--- | :--- | :--- |
| `name` | O texto do botão ou tooltip. | `'Editar'` | `['Ver', 'Editar']` |
| `buttonType`| Estilo Bootstrap (`primary`, `danger`, `transparent`). | `'primary'` | `['info', 'danger']` |
| `buttonIcon`| Classe de ícone FontAwesome. | `'fa-pencil'` | `['fa-eye', 'fa-pencil']` |
| `action` | Identificador da ação enviado no evento. | `'edit'` | `['view', 'edit']` |

> **Nota**: No tipo `group-button`, todas as propriedades acima devem ser **Arrays** com a mesma quantidade de elementos.

### 🎨 Renderização Customizada (`render`)

Você pode usar strings de template HTML simples para criar células ricas. Use `${NomeDaColuna}` para injetar dados dinâmicos da linha.

**Exemplo:**
```javascript
{ 
  column: 'Status', 
  render: '<span class="badge-status">${Status}</span>' 
}
```

### 📝 Exemplo Completo de Configuração

Abaixo, um exemplo real utilizando todas as variações de colunas (extraído do `openform.html`):

```javascript
columns: [
  // 1. Checkbox (Seleção de Linha)
  { column: 'S.L', type: 'checkbox', align: 'center', size: '20px', show: true, disabled: true },

  // 2. Inteiro Simples
  { column: 'S.L', label: 'Pedido', type: 'int', align: 'center', size: '20px', show: true },

  // 3. Texto Padrão
  { column: 'Invoice', label: 'Invoice' },

  // 4. Imagem (Avatar Circular)
  { column: 'Image', label: 'Foto', type: 'image', format: 'circle' },

  // 5. Renderização Customizada (Avatar + Texto)
  { 
    column: 'Name', 
    label: 'Nome Image', 
    render: '<div class="user-cell"><img src="${Image}" class="user-avatar" alt="${Name}"><span class="user-name">${Name} ${Amount}</span></div>' 
  },

  // 6. Data Formatada
  { column: 'Issued Date', label: 'Data Venda', type: 'date' },

  // 7. Valor Monetário
  { column: 'Amount', label: 'Valor', type: 'numeric', format: 'R$ %.3f' },

  // 8. Botão Único
  { 
    column: 'S.L', 
    label: 'Editar', 
    type: 'button', 
    name: 'Editar', 
    buttonType: 'primary', 
    buttonIcon: 'fa-light fa-pencil', 
    action: 'edit', 
    align: 'center' 
  },

  // 9. Grupo de Botões (Ações Múltiplas)
  { 
    column: 'S.L', 
    label: 'Ações', 
    type: 'group-button', 
    name: ['Visualizar', 'Editar', 'Excluir'], 
    buttonType: ['transparent ', 'transparent', 'transparent'], 
    buttonIcon: ['fa-light fa-eye', 'fa-light fa-pencil', 'fa-light fa-trash'], 
    action: ['view', 'edit', 'delete'], 
    align: 'center' 
  }
```

---

## 🔍 Filtros Customizados (`filters`)

A propriedade `filters` permite adicionar uma barra de ferramentas com filtros avançados (ranges, selects) que operam sobre as colunas.

### Estrutura do Objeto filtro

| Propriedade | Tipo | Descrição |
| :--- | :--- | :--- |
| `column` | `String` | Nome da coluna alvo (deve corresponder ao `column` definido em `columns`). |
| `type` | `String` | Tipo do filtro: `'select'` (combobox), `'number-range'` (min/max), `'date-range'` (início/fim). |
| `label` | `String` | Rótulo exibido acima ou dentro do filtro (opcional, usa o nome da coluna se omitido). |

> **Nota**: Se o `type` não for informado, o componente tenta inferir baseado no tipo da coluna correspondente.

**Exemplo de JSON:**
```javascript
filters: [
    // Filtro de Seleção (Dropdown)
    { column: 'Status', type: 'select', label: 'Situação' },

    // Filtro de Faixa de Valores (Min/Max)
    { column: 'Amount', type: 'number-range', label: 'Valor (R$)' },

    // Filtro de Faixa de Datas (Início/Fim)
    { column: 'Issued Date', type: 'date-range', label: 'Período' }
]
```

---

## 🚀 Botão de Ação Principal (`actionMainButton`)

O `actionMainButton` define um botão flutuante ou de destaque (geralmente posicionado no canto superior direito da tabela) para ações globais, como "Novo Registro".

### Estrutura do Objeto

| Propriedade | Tipo | Descrição |
| :--- | :--- | :--- |
| `name` | `String` | Texto exibido no botão. |
| `action` | `String` | Identificador da ação enviado ao evento `onButtonClick`. |
| `buttonType`| `String` | Estilo Bootstrap (ex: `'primary'`, `'success'`, `'dark'`). |
| `buttonIcon`| `String` | Classe de ícone (ex: `'fa fa-plus'`). |

**Exemplo de JSON:**
```javascript
actionMainButton: {
    name: 'Novo Pedido',
    action: 'new_order',
    buttonType: 'primary',
    buttonIcon: 'fa fa-plus'
}
```

**Comportamento:**
Ao ser clicado, dispara o evento `onButtonClick` passando o `action` definido e `value` como `null`.

---

## 🎮 API Programática (`WebrunDT`)

Os métodos podem ser chamados via objeto global `WebrunDT` passando o ID do container.

### 1. Manipulação de Dados (Novo 🌟)
- **`addRow(id, rowData)`**: Adiciona uma nova linha dinamicamente.
- **`removeRow(id, column, value)`**: Remove linhas onde `column == value` (ex: `removeRow('dt1', 'Invoice', '#1001')`).
- **`updateSqlQuery(id, query)`**: Altera a consulta SQL e recarrega a tabela no modo servidor.
- **`loadJSON(id, data)`**: Injeta um novo JSON e alterna a tabela para o modo cliente.

### 2. Controle de Ciclo de Vida
- **`refresh(id)`**: Recarrega os dados atuais.
- **`recreate(id)`**: Destrói e reinicializa a instância (mantendo o container).
- **`destroy(id)`**: Remove o componente completamente do DOM.
- **`clear(id)`**: Remove todos os dados (limpa a grade).

### 3. Interface e Filtros
- **`showColumn(id, name, visible)`**: Mostra/oculta colunas individualmente.
- **`search(id, query)`**: Busca global por texto.
- **`filterBy(id, col, v1, [v2])`**: Filtro por coluna. Suporta ranges se `v2` for informado.
- **`clearFilters(id)`**: Reseta todos os filtros e buscas da interface.

---

## ⚡ Sistema de Eventos

### `onRowClick(rowData)`
Ativo quando `clickableRows: true`. Retorna o objeto completo da linha clicada.

### `onButtonClick([action, value])`
Retorna um array com a ação definida no botão (`action`) e o valor da célula vinculada (`column`).

### `onAfterLoad(instance, trigger)`
Disparado sempre que a tabela finaliza o desenho.
- **`instance`**: Referência ao objeto `WebrunDataTable`.
- **`trigger`**: String indicando o motivo do redesenho (`'initial'`, `'search'`, `'order'`, `'page'`, `'refresh'`).

### `onCheckboxClick(instance, state, value)`
Disparado ao clicar em qualquer checkbox (linha ou cabeçalho).
- **`instance`**: Objeto `WebrunDataTable`.
- **`state`**: `Boolean`. `true` se marcado, `false` se desmarcado.
- **`value`**: Valor do checkbox. Se for o do cabeçalho, retorna `'select-all'`.

---

## 📐 Responsividade
O componente gerencia automaticamente seu tamanho através de um sistema de **ResizeObserver**. Ele monitora alterações de tamanho no container pai e recalcula a largura das colunas e a altura da tabela instantaneamente, evitando quebras de layout em janelas redimensionáveis.
