# react-xlsx-sheet-advance

## react component for work sheet

Based on xlsx package, document reference <a href="https://github.com/SheetJS/js-xlsx/">XLSX</a>

```bash
yarn add react-xlsx-sheet-advance
```
### Header attributes advancement
```js
const head = [
  {
    title: 'Sort',
    dataIndex: 'sort',
    cell: ({ value, row, dataIndex, rowNumber, colNumber }) => {
      // value: refer to value of dataIndex in the object
      // row: refer to current object
      // dataIndex: taken from the props
      // rowNumber: refer to row number of current record
      // colNumber: refer to column number of current cell

      return "Any modified result for current cell" // return result will be placed in the excel sheet
    },
  },
];
```


## Basic usage

> ExportSheet

#### DOM element

```js
import React, { PureComponent } from 'react'
import ReactDOM from 'react-dom'
import XLSX from 'xlsx'
import { ExportSheet } from 'react-xlsx-sheet'

const head = [
  { title: "Sort", dataIndex: 'sort' },
  { title: "Parent Item Id", dataIndex: 'parentItemId' },
  { title: "Stock", dataIndex: 'stock' },
  { title: "SKU Price", dataIndex: 'skuPrice' },
]

const data = [{
    "activitySales": null,
    "itemTopicStatus": 1,
    "originPrice": 40000,
    "parentItemId": "228209",
    "skuPrice": "3039",
    "sort": 111,
    "stock": 40717,
    "tax": 100,
}]

<ExportSheet
  header={head}
  fileName={`items`}
  dataSource={data}
  xlsx={XLSX}
>
  <button>Export</button>
</ExportSheet>

```

#### React Component

> When children are react components, pass prop isDOMElement={false} to distinguish native dom elements from react components

```js
import React, { PureComponent } from 'react'
import ReactDOM from 'react-dom'
import XLSX from 'xlsx'
import { ExportSheet } from 'react-xlsx-sheet'

const array = [
    ["Price", "Stock", "ID", "Name", "Range"],
    [30, 228209, 228377, "Ilyas", 40000],
    [29, 228209, 228369, "artisan", 40000],
    [30, 228209, 228377, "jsfit", 40000]
]

const App = (props) => (
  <button
    onClick={props.exportsheet}
  >React Component</button>
)

<ExportSheet
  dataType="Array-of-Arrays"
  fileName={`items`}
  dataSource={array}
  isDOMElement={false}
  xlsx={XLSX}
>
  <App />
</ExportSheet>

```

#### The data source cab be a table element

```tsx
const Table = () => (
  <table id="sheetjs">
    <tbody>
      <tr>
        <td>S</td>
        <td>h</td>
        <td>e</td>
        <td>e</td>
        <td>t</td>
        <td>J</td>
        <td>S</td>
      </tr>
      <tr>
        <td>1</td>
        <td>2</td>
        <td>3</td>
        <td>4</td>
        <td>5</td>
        <td>6</td>
        <td>7</td>
      </tr>
      <tr>
        <td>2</td>
        <td>3</td>
        <td>4</td>
        <td>5</td>
        <td>6</td>
        <td>7</td>
        <td>8</td>
      </tr>
    </tbody>
  </table>
);

const WithTable = () => {
  const [table, setTable] = useState(null);

  useEffect(() => {
    setTable(document.querySelector('#sheetjs'));
  }, []);

  return (
    <>
      <Table />
      <ExportSheet
        dataType="Table-Node-Element"
        fileName={`table`}
        tableElement={table}
        xlsx={XLSX}
      >
        <button>Table Input</button>
      </ExportSheet>
    </>
  );
};
```

> API

|      Parameter     | Defaults                             | Type                                                                       | Describe                                                                                      |
|:------------------:|--------------------------------------|----------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------|
| dataType           | Array-of-Object                      | string                                                                     | ['Array-of-Arrays', 'Array-of-Object', 'Table-Node-Element']                                  |
| header             | []                                   | Refer to the above code, when dataType="Array-of-Arrays" can not be passed | -                                                                                             |
| headerOption       | {skipHeader: false,dateNF: 'FMT 14'} | object                                                                     | xlsx, xlsm, txt, html, odsFor more, please refer to XLSX                                      |
| dataSource         | []                                   | array                                                                      | The specific value is described according to dataType                                         |
| extName            | xlsx                                 | string                                                                     | For other extensions, please refer to xlsx                                                    |
| isRequiredNameDate | true                                 | boolean                                                                    | Whether the filename has the current date                                                     |
| fileName           | -                                    | string                                                                     | file name                                                                                     |
| isDOMElement       | true                                 | boolean                                                                    | Whether children are basic dom elements (react component is false)                            |
| fileDate           | current date                         | string                                                                     | Specify a date for the file name (other values are also possible), the default is recommended |
| tableElement       | table element for input data         | HTMLTableElement                                                           | dataType="Table-Node-Element"A table element must be provided when                            |
