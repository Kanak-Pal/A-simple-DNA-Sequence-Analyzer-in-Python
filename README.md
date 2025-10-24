# A-simple-DNA-Sequence-Analyzer-in-Python
Reads FASTA files ,Counts GC content, Translates to protein, Plots base composition
def read_fasta(file_path):
    """Reads a FASTA file and returns a dictionary {header: sequence}."""
    sequences = {}
    header = None
    seq_lines = []

    try:
        with open(file_path, "r") as file:
            for line in file:
                line = line.strip()
                if not line:
                    continue

                if line.startswith(">"):  # header line
                    if header:
                        sequences[header] = "".join(seq_lines).upper()
                    header = line[1:].strip()
                    seq_lines = []
                else:
                    seq_lines.append(line)

            # Add the last sequence
            if header:
                sequences[header] = "".join(seq_lines).upper()

    except FileNotFoundError:
        print(f"Error: File '{file_path}' not found.")
        return {}

    return sequences


def gc_content(sequence):
    """Calculates GC content (%) of a DNA sequence."""
    if len(sequence) == 0:
        return 0.0
    g = sequence.count("G")
    c = sequence.count("C")
    return round((g + c) / len(sequence) * 100, 2)


if __name__ == "__main__":
    # Ask user for FASTA file path
    file_path = input("Enter the full path to your FASTA file: ").strip()
    file_path = file_path.strip('"').strip("'")  # remove accidental quotes

    fasta_data = read_fasta(file_path)

    if fasta_data:
        print("\n--- FASTA Sequences Found ---")
        for header, sequence in fasta_data.items():
            gc = gc_content(sequence)
            print(f"\n> {header}")
            print(sequence)
            print(f"Length: {len(sequence)} bases")
            print(f"GC Content: {gc}%")
    else:
        print("No sequences found or file could not be read.")
