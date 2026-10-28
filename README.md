# CMPM121S04

**Members**: Ray, Melanie, Blu, Kevin

**Code Smells**: Mysterious Name, Shotgun Surgery, Duplicate Code

## Mysterious Name

**Description**: Mysterious Names are code names that have little to no distinction from one another, which can snowball into a code roadblock that could take hours to resolve. Figuring out which variables go where through thousands of lines of code can definitely deter progress.

**Prevention and Refactoring**: For every code declaration (ie class, variable, function, etc.), you could rename them to become simple and direct in ways that explicitly state their use.

**Code**:

Before: 
```typescript
let x = 1;
let y = 2;

function calc(x:number, y:number) {
	return x + y;
}

function calc2(x:number, y:number) {
	return x - y;
}

calc1(x, y);
```
After:




## Shotgun Surgery

**Description**: When you have to make an edit and to fully implement the change you must make a bunch of small changes in a bunch of places.

**Prevention and Refactoring**: You can move all these little changes into one module or function so that there’s only one place to be changed, and in some cases, you can reduce the amount of changes. 

**Code**:
In this code in order to change the tokens you would have to change multiple places (more in the full file but here are two of the changes).

Before:
```cpp
while(getline(in, line)){
      line_count++;
      len = line.length();

      // get first token
      begin = min(line.find_first_not_of(delim, 0), len);
      end = min(line.find_first_of(delim, begin), len);
      token = line.substr(begin, end-begin);
      
      while(token!=""){  // we have a token
         // update token buffer
         if(!D.contains(token)){
            D.setValue(token, token_count);
            token_count++;
         }

         // get next token
         begin = min(line.find_first_not_of(delim, end + 1), len);
         end = min(line.find_first_of(delim, begin), len);
         token = line.substr(begin, end-begin);
      }
   }
```
After:
```cpp
size_t first_token(begin, end){
	Begin = min(line.find_first_not_of(delim, 0), len);
	end = min(line.find_first_of(delim, begin), len);
	token = line.substr(begin, end-begin);
} 

size_t next_token(begin, end){
	begin = min(line.find_first_not_of(delim, end + 1), len);
	end = min(line.find_first_of(delim, begin), len);
	token = line.substr(begin, end-begin);
}

while(getline(in, line)){
  line_count++;
  len = line.length();

  // get first token
  first_token(begin, end);
  
  while(token!=""){ // we have a token
	 // update token buffer
	 if(!D.contains(token)){
		D.setValue(token, token_count);
		token_count++;
	 }

	 // get next token
	 next_token(begin, end);
  }
 }
```

## Duplicate Code

**Description**: Duplicate code is when two functions or classes do the same thing. This can arise when you’re trying to make different objects do the same thing or have slight changes. Having duplicate code is problematic because it can cause the reader confusion about what the code is trying to do, and it makes things more complicated when trying to change both functions. “Duplication means that every time you read these copies, you need to read them carefully to see if there’s any difference.” (Fowler 72).

**Prevention and Refactoring**: When you see similar code in two places, you can prevent the smell by extracting the similar logic and putting it into a function. Fowler suggests using “Extract Function” (26).

**Code**:

Before:
```typescript
witchHelperUpgrade.addEventListener("click", () => {
  money -= initialWitchHelperPrice;
  OPC += 1;
  initialWitchHelperPrice = initialWitchHelperPrice * 1.5;
  console.log("OPC " + OPC);
 ouncesPerClick.textContent = "Ounce per click: " + OPC.toString();
 witchHelperUpgrade.textContent =
Buy a Witch Helper: $${initialWitchHelperPrice} (+1 ounce per click);
 refreshMoney();
});

dropperUpgrade.addEventListener("click", () => {
  money -= initialdropperUpgradePrice;
  OPS += 0.1;
  initialdropperUpgradePrice = initialdropperUpgradePrice * 1.5;
  console.log("OPS " + OPS);
  ouncesPerSecond.textContent = "Ounce per second: " + OPS.toString();
  dropperUpgrade.textContent =
	Buy a Dropper: $${initialdropperUpgradePrice} (+.1 ounce per second);
  refreshMoney();
});
```
After:
```typescript
interface upgrade {
  buttontext: string;
  price: number;
  newopc: number;
  newops: number;
  numbought: number;
}

const upgrades: upgrade[] = [{
  buttontext: "Buy a Witch Helper: $",
  price: 100,
  newopc: 1,
  newops: 0,
  numbought: 0,
}, {
  buttontext: "Buy a Dropper: $",
  price: 50,
  newopc: 0,
  newops: 0.1,
  numbought: 0,
}];
.
.
.
for (let i = 0; i < upgrades.length; i++) {
  const newButton = document.createElement("button");
  newButton.innerText = upgrades[i].buttontext +
    upgrades[i].price.toString() + " ";
  newButton.disabled = true;
  app.append(newButton, document.createElement("br"));
  buttons.push(newButton);

  newButton.addEventListener("click", () => {
    if (money >= upgrades[i].price) {
      money -= upgrades[i].price;
      upgrades[i].price += upgrades[i].price / 5;
      upgrades[i].numbought += 1;

      newButton.innerText = upgrades[i].buttontext + "(" +
        upgrades[i].price.toFixed(1).toString() + ") " +
        "\nOwned: (" + upgrades[i].numbought + ") ";

      ops += upgrades[i].newops;
      opc += upgrades[i].newopc;

      refreshText();
    }
  });
}
```
