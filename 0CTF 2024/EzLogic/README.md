Analyzing and decoding a hardware logic system described in `schematic.pdf`.

![Screenshot 2024-12-23 115554](https://github.com/user-attachments/assets/5c0d4364-f1d4-492c-8947-cd672b0a5f88)

Based on the information from `EzLogic_tb.vcd`, we can observe that `data_out_all` changes over time, and the variable `success=0`. => If the `success` variable becomes `1`, we might be able to find the flag.

![Screenshot 2024-12-23 120240](https://github.com/user-attachments/assets/f328be2e-3197-4863-bfd3-02f67e1090dc)
![Screenshot 2024-12-23 120301](https://github.com/user-attachments/assets/6552cef5-ee8f-4a64-8e0a-b6fad5c3ea52)


Now, let's take a look at the file `EzLogic_tb.v` in the `problem` folder to understand the flow and logic of this challenge.


```verilog
`timescale 1us / 100ns

module EzLogic_tb #(
    parameter FLAG_TO_TEST = "..........................................",
    parameter N = 42
)();
    reg clk;
    reg rst_n;
    reg valid_in;
    reg start;
    reg [7:0] data_in;
    reg [6:0] counter;
    reg [6:0] counter2;
    wire [7:0] data_out;
    wire valid_out;
    reg [0:8*N-1] data_out_all;
    wire success;

    wire [7:0] flag_test_arr [0:N-1];
    genvar i;
    generate
        for (i=0;i<N;i=i+1) begin
            assign flag_test_arr[N-1-i] = FLAG_TO_TEST[(i*8)+:8];
        end
    endgenerate

    EzLogic_top inst(
        .clk(clk),
        .rst_n(rst_n),
        .data_in(data_in),
        .valid_in(valid_in),
        .data_out(data_out),
        .valid_out(valid_out)
    );

    initial begin
        $dumpfile("EzLogic_tb.vcd");
        $dumpvars(0, EzLogic_tb);
        clk = 0;
        rst_n = 0;
        data_in = 0;
        valid_in = 0;
        counter = 0;
        counter2 = 0;
        start = 0;
        data_out_all = 0;
        #4
        rst_n = 1;
        start = 1;
        @(negedge start);
        #4
        if (success) begin
            $display("Great! You've found the correct flag!");
        end
        else begin
            $display("Haha, try again!");
        end
        #20
        $finish();
    end

    always @(posedge clk) begin
        if (start == 1) begin
            if (counter < N) begin
                counter <= counter + 1;
                data_in <= flag_test_arr[counter];
                valid_in <= 1;
            end
            else begin
                data_in <= 0;
                valid_in <= 0;
                start <= 0;
            end
        end
    end

    always @(posedge clk) begin
        if (valid_out) begin
            counter2 <= counter2 + 1;
            data_out_all[(counter2)*8 +: 8] <= data_out;
        end
    end

    wire [0:8*N-1] data_std = 'h30789d5692f2fe23bb2c5d9e16406653b6cb217c952998ce17b7143788d949952680b4bce4c30a96c753;
    assign success = (data_std == data_out_all);

    always #1 clk = ~clk;
endmodule
```
Here, we see the parameter `FLAG_TO_TEST = ".........................................."`, which appears to be our input flag.

`parameter N = 42` indicates that the flag is defined as being 42 characters long.

```verilog
wire [0:8*N-1] data_std = 'h30789d5692f2fe23bb2c5d9e16406653b6cb217c952998ce17b7143788d949952680b4bce4c30a96c753;
assign success = (data_std == data_out_all);
```

=> After the process of encoding `FLAG_TO_TEST` into the variable `data_out_all`, it is compared with the value of `data_std` to determine the value of the `success` variable.

```verilog
if (success) begin
    $display("Great! You've found the correct flag!");
end
else begin
    $display("Haha, try again!");
end
```

=> As we predicted, if the `success` variable becomes `1`, we will find the flag.

At this point, I thought about decoding the value of `data_std` to find the flag. However, after some time spent trying to decode it, I was unable to decode `data_std`.

Later, I accidentally discovered that when I replaced the value of `FLAG_TO_TEST` with `"0ops{....................................."`, progress was made.


![Screenshot 2024-12-23 122034](https://github.com/user-attachments/assets/a1e3f630-821c-47de-84c5-f29d0a054352)


After replacing and recompiling the file `EzLogic_tb.vcd`, I noticed that the first 10 bytes of `data_out_all` matched with `data_std`. => This indicates that the program encodes each character individually, and the encoding varies depending on the position of the character in the string (we can also deduce this by looking at the `schematic.pdf` file).

=> At this point, I came up with the idea of using brute force to find the flag by replacing each character of the flag sequentially and comparing the corresponding byte at that position in `data_std`.


```python
import os
import subprocess

# Parameters
flag_prefix = "0ops{"  # Known correct prefix
flag_length = 42       # Total length of the flag
characters = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz 0123456789!#$%&'()*+,-./:;<=>?@[]^_`{|}~;"
verilog_testbench = "problem/EzLogic_tb.v"
sim_command = "iverilog -s EzLogic_tb -o EzLogic.vvp ./problem/EzLogic_top_synth.v ./problem/EzLogic_tb.v ./behavioral\\ models/*.v && vvp EzLogic.vvp"

# Update FLAG_TO_TEST in the testbench
def update_testbench(flag):
    with open(verilog_testbench, "r") as file:
        lines = file.readlines()

    updated_lines = []
    for line in lines:
        if "parameter FLAG_TO_TEST" in line:
            # Update the FLAG_TO_TEST line dynamically
            updated_line = f'parameter FLAG_TO_TEST = "{flag}",\n'
            updated_lines.append(updated_line)
        else:
            updated_lines.append(line)

    with open(verilog_testbench, "w") as file:
        file.writelines(updated_lines)

    print(f"Updated FLAG_TO_TEST in testbench: {flag}")

# Extract `data_out` values from the simulation output
def get_data_out():
    result = subprocess.run(sim_command, shell=True, capture_output=True, text=True)
    # print(result)
    print("Simulation Output:\n", result.stdout)  # Debug simulation output
    data_out = []
    for line in result.stdout.splitlines():
        if "Final data_out" in line:  # Extract data_out values
            value = line.split(":")[-1].strip()
            data_out.append(value)
    return data_out

# Brute force a single character
def brute_force_character(position):
    for char in characters:
        test_flag = flag_prefix + char + "0" * (flag_length - len(flag_prefix) - 1)
        print(f"Testing flag: {test_flag}")

        # Update the testbench with the current flag
        update_testbench(test_flag)

        # Run simulation and get `data_out`
        data_out = get_data_out()
        print(data_out)
        if not data_out:
            print("Simulation error: No data_out captured.")
            continue

        # Compare the relevant `data_out` to the corresponding `data_std`
        data_std = [
            "30", "78", "9d", "56", "92", "f2", "fe", "23", "bb", "2c", "5d", "9e",
            "16", "40", "66", "53", "b6", "cb", "21", "7c", "95", "29", "98", "ce",
            "17", "b7", "14", "37", "88", "d9", "49", "95", "26", "80", "b4", "bc",
            "e4", "c3", "0a", "96", "c7", "53"
        ]  # Convert from data_std hex


        print("="*30)
        print(position)
        print(data_out[position])
        print(data_std[position])
        print("="*30)

        if data_out[position] == data_std[position]:
            print(f"Match found for character: {char}")
            return char

    print("No match found for this position. Exiting.")
    return None

# Brute force the entire flag
def brute_force_flag():
    current_flag = flag_prefix  # Start with the known prefix
    # Reference data_std once at the top of the function
    data_std = [
        "30", "78", "9d", "56", "92", "f2", "fe", "23", "bb", "2c", "5d", "9e",
        "16", "40", "66", "53", "b6", "cb", "21", "7c", "95", "29", "98", "ce",
        "17", "b7", "14", "37", "88", "d9", "49", "95", "26", "80", "b4", "bc",
        "e4", "c3", "0a", "96", "c7", "53"
    ]  # Convert from data_std hex

    for i in range(len(flag_prefix), flag_length):
        found = False  # Track if a match is found for this position
        for char in characters:
            # Build the test flag with the current guess
            test_flag = current_flag + char + "0" * (flag_length - len(current_flag) - 1)
            print(f"Testing flag: {test_flag}")

            # Update the testbench with the current flag
            update_testbench(test_flag)

            # Run simulation and get `data_out`
            data_out = get_data_out()
            if not data_out:
                print("Simulation error: No data_out captured.")
                continue

            # Debug: Print current data_out and the expected value
            print(f"Data Out for Position {i}: {data_out[i]}")
            print(f"Expected Data Std: {data_std[i]}")

            # Compare the relevant `data_out` to the corresponding `data_std`
            if data_out[i] == data_std[i]:  # Check if the current position matches
                print(f"Match found for character: {char}")
                current_flag += char  # Append the correct character
                found = True
                print(f"Current flag: {current_flag}")
                break  # Proceed to the next position

        if not found:
            print("No match found for this position. Exiting.")
            return

    print(f"Final flag: {current_flag}")

if __name__ == "__main__":
    brute_force_flag()

```

To achieve this, I will add a line to display the `data_out` characters in the `EzLogic_tb.v` file, allowing the program to identify which character has been encoded into which byte.

```verilog
$display("Final data_out: %h", data_out);
```
After running the brute-force program for some time, we were able to find the flag.

![Screenshot 2024-12-23 123203](https://github.com/user-attachments/assets/48547161-02b8-4d81-b9b5-27b62425cb3c)

> 0ops{aadc337c-b5a0-4ff0-ad94-9d1cf41956f4}



