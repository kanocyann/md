## markdown数学符号

| ∧      | \wedge                               |
| ------ | ------------------------------------ |
| ∨      | **\vee**                             |
| ¬      | **\neg**                             |
| ∘      | **\circ**                            |
| ∼      | **\sim**                             |
| ⇔      | **\Leftrightarrow**                  |
| ⇒      | **\Rightarrow**                      |
| ∀      | **\forall**                          |
| ∃      | **\exist**                           |
| {,}    | **\lbrace, \rbrace 或 {, }**         |
| ⟨,⟩    | **\lang, \rang 或 \langle, \rangle** |
| a下标b | **a_{b}**                            |



### 公式块左对齐

$$
\begin{flalign}
&\iint_D\frac{\sin y}{y}{\rm d}\sigma\\
=&\int_0^1{\rm d}y\int_{y^2}^y\frac{\sin y}{y}{\rm d}x\\
=&\int_0^1(\sin y-y\sin y){\rm d}y\\
=&\int_0^1\sin y{\rm d}y-\int_0^1y\sin y{\rm d}y\\
=&1-\sin1
&\end{flalign}
$$

在flalign环境中以&为对齐标号



### 符号之上

$$
\stackrel{洛必达}{===}\\
\mathop{x}\limits_a^b
$$

