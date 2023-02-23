---
tags:
    - NodeJS
---

https://babeljs.io/docs/en/babel-traverse



https://babeljs.io/docs/en/babel-types#memberexpression



https://astexplorer.net/



```
const traverse = require('@babel/traverse').default;
const {parse} = require('@babel/parser');
const fs = require("fs");

let apiList = [];

scan();

async function scan() {
  await readDirSync('./web-src');
  console.log(apiList.length);
  await fs.writeFileSync('./api.json', JSON.stringify(apiList))
}

async function readDirSync(path) {
  let pa = fs.readdirSync(path);
  for (let i in pa) {
    let ele = pa[i];
    let filePath = path + "/" + ele;
    let info = fs.statSync(filePath);
    if (info.isDirectory()) {
      await readDirSync(filePath);
    } else {
      let s = ele.split(".");
      let ext = s[s.length - 1];
      if (["js"].indexOf(ext) > -1) {
        // console.log("file: " + ele);
        let data = fs.readFileSync(filePath, 'utf8');
        parseFile(data, filePath);
      }
    }
  }
}

async function parseFile(code, filePath) {
  const ast = parse(code, {sourceType: 'module', plugins: ['classProperties', 'jsx', 'exportDefaultFrom']});
  traverse(ast, {
    CallExpression: function (path) {
      if (path.node.callee.object) {
        let objectName = path.node.callee.object.name;
        let functionName = path.node.callee.property.name;
        if (functionName === 'request') {
          let arguments = path.node.arguments;
          let apiItem = {
            apiPath: '',
            method: 'get',
            data: [],
            type: null,
            error: null
          };
          switch (objectName) {
            case 'AjaxAPI':
              apiItem.type = 'ajax';
              if (arguments.length <= 2) {
                switch (arguments[0].type) {
                  case 'StringLiteral':
                    apiItem.apiPath = arguments[0].value;
                    break;
                  case 'BinaryExpression':
                    apiItem.apiPath = arguments[0].right.value;
                    break;
                  case 'TemplateLiteral':
                    let str = [];
                    for (let i in arguments[0].quasis) {
                      str.push(arguments[0].quasis[i].value.raw);
                    }
                    apiItem.apiPath = str.join('');
                    break;
                  default:
                    debugger;
                    break;
                }
                if (arguments.length > 1) {
                  if (arguments[1].type === 'ObjectExpression') {
                    for (let i in arguments[1].properties) {
                      let property = arguments[1].properties[i];
                      let name = property.key.name;
                      if (name === 'method') {
                        apiItem.method = property.value.value.toLowerCase();
                      } else if (name === 'data') {
                        for (let j in property.value.properties) {
                          let subProperty = property.value.properties[j];
                          if (subProperty.type === 'ObjectProperty') {
                            apiItem.data.push(subProperty.value.name);
                          } else {
                            apiItem.error = '自动展开的属性无法处理';
                          }
                        }
                      }
                    }
                  } else {
                    debugger;
                  }
                }
              } else {
                debugger;
              }
              break;
            case 'webAPI':
            case 'WebAPI':
              apiItem.type = 'ws';
              switch (arguments[0].type) {
                case 'ObjectExpression':
                  for (let i in arguments[0].properties) {
                    let property = arguments[0].properties[i];
                    let name = property.key.name;
                    if (name === 'path') {
                      if (property.value.type === 'StringLiteral') {
                        apiItem.apiPath = property.value.value.toLowerCase();
                      } else if (property.value.type === 'BinaryExpression') {
                        apiItem.error = '拼接的apiPath无法处理：' + property.value.left.value;
                      } else {
                        debugger;
                      }
                    } else if (name === 'data') {
                      for (let j in property.value.properties) {
                        let subProperty = property.value.properties[j];
                        if (subProperty.type === 'ObjectProperty') {
                          apiItem.data.push(subProperty.value.name);
                        } else {
                          apiItem.error = '自动展开的属性无法处理';
                        }
                      }
                    }
                  }
                  break;
                default:
                  debugger;
                  break;
              }
              break;
            default:
              console.error(path.node.object.name, filePath);
              break;
          }

          if (apiItem.type) {
            if (apiItem.error) {
              console.error(`${apiItem.error}, ${apiItem.type}, ${apiItem.apiPath}, ${apiItem.data.join(',')}`);
            } else {
              apiList.push(apiItem);
            }
          }
        }
      }
    }
  });
}

```

