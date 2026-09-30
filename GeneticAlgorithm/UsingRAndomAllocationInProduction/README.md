import random

# 1. Define the Problem
# We want to maximize:
# f(x) = x * (10 - x)
# where x is the amount of resource allocated
def fitness(x):
    return x * (10 - x)


# 2. Initialize Parameters
POPULATION_SIZE = 10
MUTATION_RATE = 0.1
CROSSOVER_RATE = 0.8
GENERATIONS = 20


# 3. Create Initial Population
population = [random.randint(0, 10) for _ in range(POPULATION_SIZE)]


# 4. Evaluate Fitness
def evaluate_population(population):
    return [(individual, fitness(individual)) for individual in population]


# 5. Selection
def selection(population):
    evaluated = evaluate_population(population)

    # Sort according to fitness
    evaluated.sort(key=lambda x: x[1], reverse=True)

    # Select the best half
    selected = [individual for individual, fit in evaluated[:POPULATION_SIZE // 2]]

    return selected


# 6. Crossover
def crossover(parent1, parent2):
    if random.random() < CROSSOVER_RATE:
        # Average of two parents
        child = (parent1 + parent2) // 2
    else:
        child = parent1

    return child


# 7. Mutation
def mutation(individual):
    if random.random() < MUTATION_RATE:
        change = random.choice([-1, 1])
        individual += change

    # Keep solution within valid range
    individual = max(0, min(10, individual))

    return individual


# 8. Iteration
best_solution = None
best_fitness = float("-inf")

for generation in range(GENERATIONS):

    # Evaluate current population
    evaluated = evaluate_population(population)

    # Track best solution
    current_best = max(evaluated, key=lambda x: x[1])

    if current_best[1] > best_fitness:
        best_solution = current_best[0]
        best_fitness = current_best[1]

    # Selection
    selected = selection(population)

    # Create new population
    new_population = []

    while len(new_population) < POPULATION_SIZE:

        parent1 = random.choice(selected)
        parent2 = random.choice(selected)

        # Crossover
        child = crossover(parent1, parent2)

        # Mutation
        child = mutation(child)

        new_population.append(child)

    population = new_population

    print(
        "Generation:", generation + 1,
        "Best Solution:", best_solution,
        "Fitness:", best_fitness
    )


# 9. Output the Best Solution
print("\nFinal Best Solution:")
print("Resource Allocation:", best_solution)
print("Maximum Production/Fitness:", best_fitness)
