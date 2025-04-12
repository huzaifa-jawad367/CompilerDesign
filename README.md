# CompilerDesign

## Step 1: Define a Grammer for the language

### Top level Program Structure

```
Program            → DeclarationList
DeclarationList    → Declaration DeclarationList | ε
Declaration        → VarDeclaration | FunctionDeclaration | Statement
```

### Variable & Function Declarations with Types

```
VarDeclaration         → var Identifier TypeAnnotationOpt InitOpt ;
TypeAnnotationOpt      → : Type | ε
InitOpt                → = Expression | ε
Type                  → int | bool | float

FunctionDeclaration    → function Identifier ( ParameterListOpt ) TypeAnnotationOpt Block
ParameterListOpt       → ParameterList | ε
ParameterList          → Parameter ParameterListTail
Parameter              → Identifier TypeAnnotationOpt
ParameterListTail      → , Parameter ParameterListTail | ε
```

### Statements

```
Statement          → ExpressionStatement
                   | AssignmentStatement
                   | IfStatement
                   | WhileStatement
                   | ForStatement
                   | ReturnStatement
                   | BreakStatement
                   | ContinueStatement
                   | Block

ExpressionStatement → Expression ;

AssignmentStatement → Identifier = Expression ;

IfStatement        → if ( Expression ) Statement ElseClause
ElseClause         → else Statement | ε

WhileStatement     → while ( Expression ) Statement

ForStatement       → for ( AssignmentOpt ; ExpressionOpt ; ExpressionOpt ) Statement
AssignmentOpt      → AssignmentStatement | ε
ExpressionOpt      → Expression | ε

ReturnStatement    → return ExpressionOpt ;
BreakStatement     → break ;
ContinueStatement  → continue ;

Block              → { StatementList }
StatementList      → Statement StatementList | ε
```

Expressions (LR-friendly precedence-based structure)
```
Expression         → LogicalOr

LogicalOr          → LogicalAnd LogicalOrTail
LogicalOrTail      → || LogicalAnd LogicalOrTail | ε

LogicalAnd         → Equality EqualityTail
EqualityTail       → && Equality EqualityTail | ε

Equality           → Relational EqualityOpTail
EqualityOpTail     → ( == | != ) Relational EqualityOpTail | ε

Relational         → Additive RelationalTail
RelationalTail     → ( < | > | <= | >= ) Additive RelationalTail | ε

Additive           → Multiplicative AdditiveTail
AdditiveTail       → ( + | - ) Multiplicative AdditiveTail | ε

Multiplicative     → Unary MultiplicativeTail
MultiplicativeTail → ( * | / | % ) Unary MultiplicativeTail | ε

Unary              → ! Unary | - Unary | Primary

Primary            → Literal
                   | Identifier
                   | Identifier ( ArgumentListOpt )
                   | ( Expression )

ArgumentListOpt    → ArgumentList | ε
ArgumentList       → Expression ArgumentListTail
ArgumentListTail   → , Expression ArgumentListTail | ε
```

