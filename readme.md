<br/>
<div id="logo" align="center">
    <br />
    <h1>Voltron Mark</h1>
</div>

<div style='margin:0 auto;width:80%;'>

![Voltron](./doc/voltron.svg)

</div>

## Schema

```typescript
export interface Component {
  name: string;
}

export interface WidgetInfo {
  version: string;
  name: string;
  package: string;
  exportName: string;
}

export interface JSExpression {
  type: "JSExpression";
  value: string;
  mock?: any;
}

export interface JSFunction {
  type: "JSFunction";
  value: string;
}

export interface UtilInfo {
  name: string;
  type: "function";
  content: JSExpression | JSFunction;
}

export type JSONValue =
  | boolean
  | string
  | number
  | null
  | undefined
  | JSONArray
  | JSONObject;

export type JSONArray = JSONValue[];
export interface JSONObject {
  [key: string]: JSONValue;
}

export interface Schema {
  // 版本号
  version: string;
  // 工具
  utils?: UtilInfo[];
  // 常量
  constants?: JSONObject;
  // 样式
  css?: string;
  // 组件包集合
  widgetMaps: WidgetInfo[];
  // 组件树形
  widgetTrees: ContainerSchema[];
  // 基础信息
  meta?: Record<string, any>;
}

export interface JSSlot {
  type: "JSSlot";
  title?: string;
  params?: string[];
  value?: NodeData[] | NodeData;
  name?: string;
}

export type CompositeValue =
  | JSONValue
  | JSExpression
  | JSFunction
  | JSSlot
  | CompositeArray
  | CompositeObject;
export type CompositeArray = CompositeValue[];
export interface CompositeObject {
  [key: string]: CompositeValue;
}

export type PropsMap = CompositeObject;
export type PropsList = Array<{
  spread?: boolean;
  name?: string;
  value: CompositeValue;
}>;

export type NodeData = NodeSchema | JSExpression | DOMText;
export type NodeDataType = NodeData | NodeData[];

export function isDOMText(data: any): data is DOMText {
  return typeof data === "string";
}

export type DOMText = string;

export interface NodeSchema {
  id?: string;
  name: string;
  props?: {
    children?: NodeData | NodeData[];
  } & PropsMap;
  leadingComponents?: string;
  condition?: CompositeValue;

  loop?: CompositeValue;
  loopArgs?: [string, string];
  children?: NodeData | NodeData[];
  isLocked?: boolean;

  conditionGroup?: string;
  title?: string;
  ignore?: boolean;
  locked?: boolean;
  hidden?: boolean;
  isTopFixed?: boolean;
}

export interface ContainerSchema extends NodeSchema {
  componentName: string;
  fileName: string;
  meta?: Record<string, unknown>;
  state?: {
    [key: string]: CompositeValue;
  };
  methods?: {
    [key: string]: JSExpression | JSFunction;
  };
  lifeCycles?: {
    [key: string]: JSExpression | JSFunction;
  };
  css?: string;
  defaultProps?: CompositeObject;
}

```
