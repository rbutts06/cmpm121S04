# CMPM121S04

**Members**: Ray, Melanie, Blu, Kevin

**Code Smells**: Mysterious Name, Shotgun Surgery, Duplicate Code

Mysterious Name:
Description: 
Mysterious Names are code names that have little to no distinction from one another, which can snowball into a code roadblock that could take hours to resolve. Figuring out which variables go where through thousands of lines of code can definitely deter progress.
Prevention and Refactoring:
For every code declaration (ie class, variable, function, etc.), you could rename them to become simple and direct in ways that explicitly state their use.
Code:
		x = 5;
		X = 8;
		print(x + X);

Shotgun Surgery:
Description: 
When you have to make an edit and to fully implement the change you must make a bunch of small changes in a bunch of places.
Prevention and Refactoring:
You can move all these little changes into one module or function so that there’s only one place to be changed, and in some cases, you can reduce the amount of changes. 
Code:
In this code in order to change the tokens you would have to change multiple places (more in the full file but here are two of the changes).
while( getline(in, line) )  {
      line_count++;
      len = line.length();

      // get first token
      begin = min(line.find_first_not_of(delim, 0), len);
      end   = min(line.find_first_of(delim, begin), len);
      token = line.substr(begin, end-begin);
      
      while( token!="" ){  // we have a token
         // update token buffer
         if(!D.contains(token)){
            D.setValue(token, token_count);
            token_count++;
         }

         // get next token
         begin = min(line.find_first_not_of(delim, end+1), len);
         end   = min(line.find_first_of(delim, begin), len);
         token = line.substr(begin, end-begin);
      }
   }



Duplicate Code: 
Description:
Duplicate code is when two functions or classes do the same thing. This can arise when you’re trying to make different objects do the same thing or have slight changes. Having duplicate code is problematic because it can cause the reader confusion about what the code is trying to do, and it makes things more complicated when trying to change both functions. “Duplication means that every time you read these copies, you need to read them carefully to see if there’s any difference.” (Fowler 72).
Prevention and Refactoring:
When you see similar code in two places, you can prevent the smell by extracting the similar logic and putting it into a function. Fowler suggests using “Extract Function” (26).
Code:
witchHelperUpgrade.addEventListener("click", () => {
  money -= initialWitchHelperPrice;
  OPC += 1;
  initialWitchHelperPrice = initialWitchHelperPrice * 1.5;
  console.log("OPC " + OPC);
  ouncesPerClick.textContent = "Ounce per click: " + OPC.toString();
  witchHelperUpgrade.textContent =
    `Buy a Witch Helper: $${initialWitchHelperPrice} (+1 ounce per click)`;
  refreshMoney();
});

dropperUpgrade.addEventListener("click", () => {
  money -= initialdropperUpgradePrice;
  OPS += 0.1;
  initialdropperUpgradePrice = initialdropperUpgradePrice * 1.5;
  console.log("OPS " + OPS);
  ouncesPerSecond.textContent = "Ounce per second: " + OPS.toString();
  dropperUpgrade.textContent =
    `Buy a Dropper: $${initialdropperUpgradePrice} (+.1 ounce per second)`;
  refreshMoney();
});
