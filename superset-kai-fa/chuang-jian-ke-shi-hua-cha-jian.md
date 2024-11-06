# 创建可视化插件

Superset 中的可视化是用 JavaScript 或 TypeScript 实现的。Superset 预装了几种可视化类型（以下简称 “viz 插件”），可在 superset-frontend/plugins 目录下找到。可视化插件在 superset-frontend/src/visualizations/presets/MainPreset.js 中添加到应用程序中。



## 创建一个简单的可视化插件

要开始使用，您需要 Superset Yeoman 生成器。建议使用您正在使用的 Superset 版本所附带的模板版本。安装方法如下：

```
npm i -g yo
cd superset-frontend/packages/generator-superset
npm i
npm link
```

之后，您就可以继续创建 viz 插件了。为您的 viz 插件创建一个新目录，前缀为 superset-plugin-chart，然后运行 Yeoman 生成器

```
mkdir /tmp/superset-plugin-chart-hello-world
cd /tmp/superset-plugin-chart-hello-world
```

初始化可视化插件：

```
yo @superset-ui/superset
```

之后，生成器会问几个问题（默认值应该没问题）：

```
$ yo @superset-ui/superset
     _-----_     ╭──────────────────────────╮
    |       |    │      Welcome to the      │
    |--(o)--|    │    generator-superset    │
   `---------´   │        generator!        │
    ( _´U`_ )    ╰──────────────────────────╯
    /___A___\   /
     |  ~  |
   __'.___.'__
 ´   `  |° ´ Y `
? Package name: superset-plugin-chart-hello-world
? Description: Hello World
? What type of chart would you like? Time-series chart
   create package.json
   create .gitignore
   create babel.config.js
   create jest.config.js
   create README.md
   create tsconfig.json
   create src/index.ts
   create src/plugin/buildQuery.ts
   create src/plugin/controlPanel.ts
   create src/plugin/index.ts
   create src/plugin/transformProps.ts
   create src/types.ts
   create src/SupersetPluginChartHelloWorld.tsx
   create test/index.test.ts
   create test/__mocks__/mockExportString.js
   create test/plugin/buildQuery.test.ts
   create test/plugin/transformProps.test.ts
   create types/external.d.ts
   create src/images/thumbnail.png
```

要构建可视化插件，请运行以下命令：

```
npm i --force
npm run build
```

或者，要在开发模式下运行可视化插件（每当进行更改时重建），请使用以下命令启动开发服务器：

```
npm run dev
```

要将软件包添加到 Superset，请进入 Superset 源文件夹中的 superset-frontend 子目录，运行以下命令：

```
npm i -S /tmp/superset-plugin-chart-hello-world
```

如果您将包发布到 npm，您自然也可以直接从那里安装。之后编辑 superset-frontend/src/visualizations/presets/MainPreset.js 并进行以下更改：

```
import { SupersetPluginChartHelloWorld } from 'superset-plugin-chart-hello-world';
```

导入 viz 插件，然后将以下内容添加到传递给 plugins 属性的数组中：

```
new SupersetPluginChartHelloWorld().configure({ key: 'ext-hello-world' }),
```

之后，在运行 Superset（例如开发服务器）时，就会显示 viz 插件：

```
npm run dev-server
```



## 可视化插件讲解

上面的内容来自官方文档翻译，经过上面的步骤我们完成了可视化插件的创建以及如何添加到superset中，接下来将详细讲解可视化插件文件夹下包含哪些内容，逐步揭开superset的面纱，如何开发自己的可视化插件。

进入到可视化插件目录，我们可以看到这样的目录结构：

```
.
├── buildQuery.ts
├── controlPanel.tsx
├── EchartsPie.tsx
├── images
│   ├── Pie1.jpg
│   ├── Pie2.jpg
│   ├── Pie3.jpg
│   ├── Pie4.jpg
│   └── thumbnail.png
├── index.ts
├── transformProps.ts
└── types.ts
```

### **buildQuery.ts**

定义查询数据的方式。每次打开图表时，图表会向服务器发起请求&#x20;http://example.com/api/v1/chart/data请求图表的数据，可以看到请求负载为：

<figure><img src="../.gitbook/assets/image (2) (1).png" alt=""><figcaption></figcaption></figure>

buildQuery.ts的作用是将图表配置信息转换为QueryContext，服务器接收到queryContext去处理查询，然后将查询到的数据返回给图表。



### **transformProps.ts**

处理服务器返回的查询数据，转换为组件内部可使用的数据格式。



### **controlPanel.tsx**

用于定义superset创建图表时左侧的控制面板，它将设置传递给主组件，由主组件创建数据可视化查询。

<figure><img src="../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

该文件主要包括一个对象，其中包含可视化插件中使用的每个设置的数组。Superset 将这些设置作为props传递给插件，用于插件中的数据查询或其他可视化选项。让我们看看配置对象是如何定义控制面板元素的：

```typescript
const config: ControlPanelConfig = {
  controlPanelSections: [
    {
      label: t('Query'),
      expanded: true,
      controlSetRows: [
      ...     //此处设置控件
      ]
    },
    ...
  ]
  ...
}
```

这里有两种控件： 默认控件和自定义控件

#### **默认控件**

它是superset中预置的控件

```javascript
{
	controlSetRows: [
		["adhoc_filters"],
		//  or 
		["series"]
	]
}
```

建议查看superset中的现有控件，并比较相关控制面板的编辑方式。所有现有默认的控件列表可在以下文件中找到：

```
/superset-frontend/packages/superset-ui-chart-controls/src/shared-controls/sharedControls.tsx
```

可以看到有这些控件：

```typescript
export default {
  metrics: dndAdhocMetricsControl,
  metric: dndAdhocMetricControl,
  datasource: datasourceControl,
  viz_type,
  color_picker,
  metric_2: dndAdhocMetricControl2,
  linear_color_scheme,
  secondary_metric: dndSecondaryMetricControl,
  groupby: dndGroupByControl,
  columns: dndColumnsControl,
  granularity,
  granularity_sqla: dndGranularitySqlaControl,
  time_grain_sqla,
  time_range,
  row_limit,
  limit,
  timeseries_limit_metric: dndSortByControl,
  orderby: dndSortByControl,
  order_desc,
  series: dndSeriesControl,
  entity: dndEntityControl,
  x: dndXControl,
  y: dndYControl,
  size: dndSizeControl,
  y_axis_format,
  x_axis_time_format,
  adhoc_filters: dndAdhocFilterControl,
  color_scheme,
  time_shift_color,
  series_columns: dndColumnsControl,
  series_limit,
  series_limit_metric: dndSortByControl,
  legacy_order_by: dndSortByControl,
  truncate_metric,
  x_axis: dndXAxisControl,
  show_empty_columns,
  temporal_columns_lookup,
  currency_format,
  sort_by_metric,
};

```

我们可从官方源码中看到控件是如何定义的，例如行限制控件：

```typescript
const row_limit: SharedControlConfig<'SelectControl'> = {
  type: 'SelectControl',
  freeForm: true,
  label: t('Row limit'),
  clearable: false,
  mapStateToProps: state => ({ maxValue: state?.common?.conf?.SQL_MAX_ROW }),
  validators: [
    legacyValidateInteger,
    (v, state) => validateMaxValue(v, state?.maxValue || DEFAULT_MAX_ROW),
  ],
  default: 10000,
  choices: formatSelectOptions(ROW_LIMIT_OPTIONS),
  description: t(
    'Limits the number of the rows that are computed in the query that is the source of the data used for this chart.',
  ),
};
```

所以，只需使用共享控件中的预设选项，你就能建立自己的控制面板。只需选择你需要的控件名称，然后像这样设置即可：

```typescript
const config: ControlPanelConfig = {
  controlPanelSections: [
    {
      label: t('Query'),
      expanded: true,
      controlSetRows: [
        ['entity'],  
        ['adhoc_filters'],
        ['row_limit'], 
				//... 
				//其他的控件
				//...       
      ],
    },
    ...
  ]
}
```



#### **自定义控件**

如果默认字段不满足您的要求或需要编辑，也可以直接在控制面板中创建。

例如： 你想在控制面板中添加另一个复选框，并更改说明，以便让用户清楚地了解其功能。默认复选框不允许这样做：

```
{
        controlSetRows: [
			["CheckboxControl"],   //默认的控件   
	]	
}
```

你可以这样做：

```typescript
{
	controlSetRows: [
			[       type: 'CheckboxControl',   // 
			       label: t('Sort Descending'),
			     default: true,
		         description: t('My new Description'), //Changes possible
                          visibility: ({ controls }) =>
                           Boolean(
				controls?.timeseries_limit_metric.value &&
		        !isEmpty(controls?.timeseries_limit_metric.value),
        )
    ]	
}
```



























### **EchartsPie.tsx**

定义的图表组件。



### **index.ts**

组合图表组件，配置组件的元数据。

<figure><img src="../.gitbook/assets/image (3) (1).png" alt=""><figcaption></figcaption></figure>

这些元数据我们能够在superset新建图表中看到：

<figure><img src="../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

### **images**

这个目录用于存放图表的缩略图。

<figure><img src="../.gitbook/assets/image (1) (1).png" alt=""><figcaption></figcaption></figure>



### **types.ts**

定义组件内参数的类型















