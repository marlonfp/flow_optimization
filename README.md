English

Distribution network optimization model
Version: 1.0

Date: 2023-11-24

Author: Marlon Fantinel Pedroso

Description:

This model optimizes a distribution network, considering the transportation costs between the factory and the distribution centers (DCs), and between the DCs and the customers.

Inputs:

Excel file with the following sheets:
data: contains the volumes to be transported between DCs and customers.
dist_fab_CDS: contains the distances between the factory and the DCs.
Outputs:

Excel file with the following sheet:
optimized_network: contains the relationship between customers and the best DCs considering the total flow (factory->DC->Customer).
How to use:

Import the code into your development environment.
Open the Excel file with the inputs and save it in an accessible location.
Change the variables arquivo_excel, aba_volumes, arquivo_resultado to point to the input and output files, respectively.
Run the code.
Example:

Python
# Import the code
import numpy as np
import pandas as pd
from scipy.optimize import linprog

# Change the variables to point to the input and output files
arquivo_excel = 'optimizacao.xlsx'
aba_volumes = 'dados'
arquivo_resultado = 'resultado_malha.xlsx'

# Run the code
print("Executando modelagem")
with pd.ExcelWriter(arquivo_resultado, engine='openpyxl') as writer:
    resultado = linprog(funcao_c, A_ub=funcao_a, b_ub=funcao_b, bounds=bounds_malha)

    # Open the results
    viagens_por_cd = resultado.x[:num_cds * num_clientes]
    viagens_por_cliente = np.reshape(viagens_por_cd, (num_clientes, num_cds))

    melhores_cds = [id_cds[np.argmax(viagens_por_cliente[i])] for i in range(num_clientes)]
    resultados_df = pd.DataFrame({'Cliente_ID': id_clientes, 'Melhor CD': melhores_cds})

    # Export the relationship between customers and the best DCs considering the total flow (factory->DC->Customer)
    resultados_df.to_excel(writer, sheet_name="malha_optimizada", index=False)
    writer.save()
    print('Modelagem completa com sucesso')
Use o código com cuidado. Saiba mais
Explanation of the code:

The linprog() function from the scipy.optimize module is used to solve linear programming problems. The function takes as input the objective function, the constraints, and the bounds of the variables.

In the case of this model, the objective function is to minimize the total distance traveled between the factory and the customers. The constraints are as follows:

The total flow from each customer must be equal to its demand.
Each DC can serve at most one customer demand.
The bounds of the variables are as follows:

The flow between each DC and each customer must be an integer between 0 and 1.
The code starts by importing the necessary libraries. Then, it opens the Excel file with the inputs and defines the input and output variables.

The main() function executes the optimization model. The function first calculates the values of the objective function, the constraints, and the bounds of the variables. Then, it calls the linprog() function to solve the problem.

The linprog() function returns the vector of solutions. The code then separates the vector of solutions into two parts: one part for the flows between DCs and customers, and another part for the best DCs for each customer.

Finally, the code exports the results to an Excel file.

References:

SciPy documentation for linprog: https://docs.scipy.org/doc/scipy/reference/generated/scipy.optimize.linprog.html
Portuguese

Modelo de otimização de malha de distribuição
Versão: 1.0

Data: 2023-11-24

Autor: Marlon Fantinel Pedroso


