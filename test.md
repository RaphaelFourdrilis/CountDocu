---
title: Documentation syntax
---

# Basics

## Titles

Titles can be written as such: `# My title` which produces:

# My title

## Code

- Inline code:

	To include code, you can chose between several options. Inline code uses backticks (\`): `int a = 5`.

- Code blocks:

	You can also simply indent the text destined to be included as code by 4 spaces:
	

    int main(int ac, char **av);


If you also want to have syntax highlighting, you need to use *fenced code blocks* (\`\`\`language ... \`\`\`):

\`\`\`cpp

int main(int ac, char **av) { \
	printf("Lalala"); \
	return 0; \
} \
\`\`\`

produces:


```cpp
int main(int ac, char **av) {
	printf("Lalala");
	return 0;
}
```

## Math

You can include inline formulas by enclosing them between `$` signs:

`$x = x^2$` produces $x = x^2$

For more complicated formulas, use double `$` signs:

`$$\sum_{i=0}^n i^2 = \frac{(n^2+n)(2n+1)}{6}$$` produces:

$$\sum_{i=0}^n i^2 = \frac{(n^2+n)(2n+1)}{6}$$
