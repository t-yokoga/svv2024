# 第4回 時相論理

この回では，教科書の第4章 "Temporal Logic" について説明します．

!!! 概要
    - 時相論理とは
    - 計算木論理 CTL\*
    - CTL\* の文法と意味論
    - その他の時相論理
    - 状態集合との対応付け
    - 時相論理と公平性

## 時相論理とは

Temporal logic is a formalism to specify the dynamic behavior of systems, modeled as
Kripke structures. In the temporal logics that we will consider, a formula might specify that
a property holds in the next time, after one computation step; it can specify that eventually
some designated state is reached, or that an error state is never entered. Properties like
next time, eventually, and never are specified using special temporal operators. Temporal
logic also contains path quantifiers to relate temporal properties to the paths of the Kripke
structure. The operators and quantifiers can be nested and combined with Boolean connectives.
We focus on the powerful logic CTL* [121, 123, 199) and its important fragments
computation tree logic (CTL) [46, 121, 198), universal CTL\* (ACTL\*) [249, 354) and
linear temporal logic (LTL) [411,412), whose specific properties can be exploited in model
checking algorithms.

## 計算木論理 CTL\*

The intuitive semantics of computation tree logic is based on the notion of computation
trees. Given an initial state so in a Kripke structure M, the tree is formed by unwinding the
structure into a tree with root so, as illustrated in figure 4.1. The computation tree shows all
of the possible executions starting from the initial state. We require the transition relation
of M to be left-total; that is, each state has a successor. Thus, all branches of the tree are
infinite.

The temporal logic CTL\* defines properties of computation trees, and thus, of the underlying
Kripke structures. CTL\* formulas are composed of path quantifiers and temporal
operators. CTL\* has two path quantifiers:

- $\textbf{A}\varphi$ "all computation paths"

    This means that all paths from a given state have property $\varphi$.

- $\textbf{E}\varphi$ "there exists a computation path"

    This means that at least one path from a given state has property $\varphi$.

Path quantifiers are used in a particular state to specify that all of the paths or some of the
paths starting at that state have property q>.A s we show below, combinations of multiple A
and E quantifiers can describe the branching structure in the computation tree. The temporal
operators describe properties that hold along a given infinite path through the tree. There
are five basic operators. Their intuitive meaning is presented below, assuming p and q are
formulas describing state properties (see also figure 4.2):

- $\textbf{X}p$ "next time p"
Intuitively, this requires that proposition p holds on the second state of the path.
- $\textbf{F}p$ "eventually p" or "in the future p"
This is used to assert that property p holds at some state on the path.
- $\textbf{G}p$ "always p" or "globally p"
This specifies that proposition p holds at every state on the path.
- $p\textbf{U}q$ "p until q"
The U operator is a bit more complicated since it is used to combine two properties.
It holds if there is a state on the path where the second property q holds, and at every
preceding state on the path (if it exists), the first property p holds.
- $p\textbf{R}q$ "p release q"
This is the logical dual of the U operator. It requires that the second property q holds
along the path up to and including the first state where the first property p holds.
However, the first property pis not required to eventually hold.

#### Example 4.1
Let us illustrate the CTL* semantics informally on the example of figure 4.1.
Let $\pi_1$ and $\pi_2$ denote the leftmost and rightmost paths, respectively. On $\pi_1$, property $b$ holds
in every state, and thus $\textbf{G}b$ holds true. Formally, we write $\pi_1 \models \textbf{G}b$. By contrast, it is
easy to see that $\pi_2 \nVDash \textbf{G}b$. Thus, one but not all paths from the initial state satisfy $\textbf{G}b$, and
hence, $s_0 \models \textbf{EG}b$, but $s_0 \nVDash \textbf{AG}b$. Similarly, it is easy to see that $s_0 \models \textbf{EXX}(a \wedge b)$, but $s_0 \nVDash \textbf{EXAX}(a \wedge b)$.

From this informal introduction to CTL\* we see an important difference between path
quantifiers and temporal operators: while path quantifiers describe properties of a state (for
example, "does a certain path start in this state?"), temporal operators describe properties
of paths (for example, "can a certain state be reached along this path?"). In the following
section this distinction will give rise to the notion of path formulas and state formulas.

## CTL\* の文法と意味論
