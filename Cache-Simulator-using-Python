from collections import OrderedDict
import random

class CacheSimulator:
    def _init_(self, cache_capacity, block_size, main_memory_capacity, associativity, replacement_policy):
        self.cache_capacity = cache_capacity
        self.block_size = block_size
        self.main_memory_capacity = main_memory_capacity
        self.associativity = associativity
        self.replacement_policy = replacement_policy
        self.num_sets, self.set_size = self.calculate_sets()

        self.cache = {i: OrderedDict() for i in range(self.num_sets)}
        self.hits = 0
        self.misses = 0
        self.instructions = []

    def calculate_sets(self):
        if self.associativity == 'fully_associative':
            num_sets = 1
            set_size = self.cache_capacity // self.block_size
        elif 'set_associative' in self.associativity:
            num = int(self.associativity.split('-')[-1])
            num_sets = self.cache_capacity // (self.block_size * num)
            set_size = num
        else:
            num_sets = self.cache_capacity // self.block_size
            set_size = 1
        return num_sets, set_size

    def access_memory(self, address, write):
        index = (address // self.block_size) % self.num_sets
        tag = address // (self.block_size * self.num_sets)
        offset = address % self.block_size
        operation = "write" if write else "read"

        if tag in self.cache[index]:
            self.hits += 1
            hit = True
            # Move the recently accessed block to the end if LRU
            if self.replacement_policy == 'lru':
                self.cache[index].move_to_end(tag)
        else:
            self.misses += 1
            hit = False
            if len(self.cache[index]) >= self.set_size:
                self.evict(index)
            self.cache[index][tag] = None  # Simulate loading the block

        self.instructions.append({
            'address': address,
            'hit': hit,
            'write': write,
            'tag': tag,
            'index': index,
            'offset': offset
        })

    def evict(self, index):
        if self.replacement_policy == 'random':
            evict_tag = random.choice(list(self.cache[index].keys()))
        elif self.replacement_policy == 'fifo':
            evict_tag = next(iter(self.cache[index]))
        elif self.replacement_policy == 'lru':
            evict_tag = next(iter(self.cache[index]))
        self.cache[index].pop(evict_tag)

    def get_stats(self):
        return {
            'Hits': self.hits,
            'Misses': self.misses,
            'Hit Rate': self.hits / (self.hits + self.misses) if self.hits + self.misses > 0 else 0
        }

    def get_instructions(self):
        return self.instructions

    def print_cache_state(self):
        print("Current Cache State:")
        for index, blocks in self.cache.items():
            print(f"Set {index}: {list(blocks.keys())}")
def main():
    print("Cache Simulator Configuration")
    
    def get_int_input(prompt):
        while True:
            try:
                return int(input(prompt))
            except ValueError:
                print("Invalid input. Please enter a valid integer.")

    cache_capacity = get_int_input("Enter Cache Capacity (bytes): ")
    block_size = get_int_input("Enter Block Size (bytes): ")
    main_memory_capacity = get_int_input("Enter Main Memory Capacity (bytes): ")

    # Handling associativity input properly
    while True:
        associativity_input = input("Enter Associativity (direct, set_associative, fully_associative): ")
        if associativity_input == "set_associative":
            n = get_int_input("Enter the number of lines per set (N for set_associative-N): ")
            associativity = f"set_associative-{n}"
            break
        elif associativity_input in ["direct", "fully_associative"]:
            associativity = associativity_input
            break
        else:
            print("Invalid associativity type. Please choose direct, set_associative, or fully_associative.")

    replacement_policy = input("Enter Replacement Policy (random, lru, fifo): ")

    num_addresses = get_int_input("Enter number of memory addresses to simulate: ")

    try:
        simulator = CacheSimulator(cache_capacity, block_size, main_memory_capacity, associativity, replacement_policy)
    except ValueError as e:
        print(e)
        return

    print("\nMapping Memory Addresses:")
    for i in range(num_addresses):
        address_input = get_int_input(f"Enter memory address {i + 1}: ")
        write_input = input("Is it a write operation? (Y/N): ").upper()
        write = write_input == 'Y'
        simulator.access_memory(address_input, write)

    print("\nSimulation Statistics:")
    stats = simulator.get_stats()
    print(f"Hits: {stats['Hits']}")
    print(f"Misses: {stats['Misses']}")
    print(f"Hit Rate: {stats['Hit Rate']:.2%}")

    print("\nList of Previous Instructions:")
    for instruction in simulator.get_instructions():
        hit_status = "Hit" if instruction['hit'] else "Miss"
        write_status = "Write" if instruction['write'] else "Read"
        print(f"Address={instruction['address']}, {hit_status}, {write_status}, Tag={instruction['tag']}, Index={instruction['index']}, Offset={instruction['offset']}")

    simulator.print_cache_state()

if _name_ == "_main_":
    main()
