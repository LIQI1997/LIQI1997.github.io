# TypeScript

```ts
export enum EColors {
    Red = 0,
    Blue = 7
}

export enum EType {
    Success = 'Success',
    Fail = 'Fail'
}

let value = 0;
value = 7;

if (value === EColors.Blue) {
  console.log(EColors[value])
}

function print(title: string) {
  console.log(title)
}

print(EType.Success)
```