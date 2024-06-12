# Finite Element Discrete Variable Representation for a simple 1D ODE in Rust
## Running the code
To run the code, first install [rust](https://www.rust-lang.org/learn/get-started) and then follow [these instructions](https://docs.rs/GSL/latest/rgsl/index.html) to download GSL.  
Next clone this repository and navigate to the `cargo.toml` file, change "accelerate" in lapack's features list to one of the following:
|  OS | feature |
|-----|-------|
| MacOS | `"accelerate"` |
| Ubuntu | `"netlib"` |
| Windows | ??? |

Even though I'm not using this exact crate, look [here](https://docs.rs/nalgebra-lapack/latest/nalgebra_lapack/) for more information.

Once this is done, run `cargo test test_solving_ode_at_once --release -- --nocapture`, 
`cargo test many_interval_ode_solving --release -- --nocapture`, `cargo test ode_solving_via_bridges --release -- --nocapture` to run any of the programs.  You can also run 
`cargo test --release -- --nocapture --test-threads=1` to run all tests.  If you want to modify the code and play around with the functions, you'll likely want to comment 
out `println!("\nSystems of eqs:"); matrix.print_fancy_with(&result, &vector);` as it was largely meant for debugging and does not affect the rest of the program.

## Systems of Equations Visualizations
Largely to help with debugging, the systems of equations solved are printed out in the terminal as grids of colored blocks where each 
block: `██` is an element in the matrix/vector, the hue represents the complex phase (+1 = red, +i = green, -1 = cyan, -i = purple), and the brightness represents the magnitude on a log10 scale.
Additionally, zeros are displayed as grid markings: `┼─`.  Look at the example below to see how it is printed out

<img width="266" alt="Screenshot 2024-06-12 at 13 50 34" src="https://github.com/TomjWolcott/rust_fedvr_for_simple_ode/assets/134332655/625f8fab-092c-4945-9a65-b6399cbfc247">

This shows $`M_{i,j} * c_j = b_i`$ where $`M_{i,j}`$ and $`b_i`$ are computed and $`c_j`$ is solved for.  $`M_{i,j}`$ is the N x N block on the left, 
$`c_j`$ is in the middle and $`b_i`$ is on the right.  Also note that the rows of the matrix are indexed by $`i`$ and the columns are indexed by $`j`$.

## Outputs
All of these were done on the range 0.0 to 5.0 with 80 points total optionally split into 4 intervals with the initial condition of Ψ(1.0) = 1.0.

### Single DVR
```
Ψ(______) =       Expected       vs       Computed      
Ψ(0.0000) =   0.7552 + 0.9589i   vs   0.7552 + 0.9589i   -- error: 2.1069e-12
Ψ(0.0500) =   0.7564 + 0.9567i   vs   0.7564 + 0.9567i   -- error: 2.0064e-12
Ψ(0.1000) =   0.7599 + 0.9501i   vs   0.7599 + 0.9501i   -- error: 1.9107e-12
Ψ(0.1500) =   0.7658 + 0.9390i   vs   0.7658 + 0.9390i   -- error: 1.8180e-12
Ψ(0.2000) =   0.7740 + 0.9236i   vs   0.7740 + 0.9236i   -- error: 1.7255e-12
Ψ(0.2500) =   0.7843 + 0.9035i   vs   0.7843 + 0.9035i   -- error: 1.6304e-12
Ψ(0.3000) =   0.7965 + 0.8789i   vs   0.7965 + 0.8789i   -- error: 1.5332e-12
Ψ(0.3500) =   0.8106 + 0.8496i   vs   0.8106 + 0.8496i   -- error: 1.4396e-12
Ψ(0.4000) =   0.8262 + 0.8155i   vs   0.8262 + 0.8155i   -- error: 1.3457e-12
Ψ(0.4500) =   0.8431 + 0.7765i   vs   0.8431 + 0.7765i   -- error: 1.2499e-12
Ψ(0.5000) =   0.8610 + 0.7325i   vs   0.8610 + 0.7325i   -- error: 1.1521e-12
Ψ(0.5500) =   0.8796 + 0.6834i   vs   0.8796 + 0.6834i   -- error: 1.0490e-12
Ψ(0.6000) =   0.8985 + 0.6291i   vs   0.8985 + 0.6291i   -- error: 9.4750e-13
Ψ(0.6500) =   0.9172 + 0.5695i   vs   0.9172 + 0.5695i   -- error: 8.4433e-13
Ψ(0.7000) =   0.9353 + 0.5045i   vs   0.9353 + 0.5045i   -- error: 7.4014e-13
Ψ(0.7500) =   0.9523 + 0.4340i   vs   0.9523 + 0.4340i   -- error: 6.2652e-13
Ψ(0.8000) =   0.9677 + 0.3581i   vs   0.9677 + 0.3581i   -- error: 5.1044e-13
Ψ(0.8500) =   0.9808 + 0.2766i   vs   0.9808 + 0.2766i   -- error: 3.9336e-13
Ψ(0.9000) =   0.9910 + 0.1897i   vs   0.9910 + 0.1897i   -- error: 2.6743e-13
Ψ(0.9500) =   0.9976 + 0.0975i   vs   0.9976 + 0.0975i   -- error: 1.3753e-13
Ψ(1.0000) =  1.0000 + -0.0000i   vs   1.0000 + 0.0000i   -- error: 2.2209e-15
Ψ(1.0500) =  0.9974 + -0.1025i   vs  0.9974 + -0.1025i   -- error: 1.4811e-13
Ψ(1.1000) =  0.9890 + -0.2096i   vs  0.9890 + -0.2096i   -- error: 2.9875e-13
Ψ(1.1500) =  0.9741 + -0.3211i   vs  0.9741 + -0.3211i   -- error: 4.5807e-13
Ψ(1.2000) =  0.9518 + -0.4365i   vs  0.9518 + -0.4365i   -- error: 6.2949e-13
Ψ(1.2500) =  0.9214 + -0.5551i   vs  0.9214 + -0.5551i   -- error: 8.0955e-13
Ψ(1.3000) =  0.8822 + -0.6764i   vs  0.8822 + -0.6764i   -- error: 1.0005e-12
Ψ(1.3500) =  0.8332 + -0.7995i   vs  0.8332 + -0.7995i   -- error: 1.2001e-12
Ψ(1.4000) =  0.7740 + -0.9236i   vs  0.7740 + -0.9236i   -- error: 1.4121e-12
Ψ(1.4500) =  0.7037 + -1.0475i   vs  0.7037 + -1.0475i   -- error: 1.6396e-12
Ψ(1.5000) =  0.6219 + -1.1702i   vs  0.6219 + -1.1702i   -- error: 1.8772e-12
Ψ(1.5500) =  0.5281 + -1.2903i   vs  0.5281 + -1.2903i   -- error: 2.1292e-12
Ψ(1.6000) =  0.4218 + -1.4066i   vs  0.4218 + -1.4066i   -- error: 2.3919e-12
Ψ(1.6500) =  0.3030 + -1.5173i   vs  0.3030 + -1.5173i   -- error: 2.6688e-12
Ψ(1.7000) =  0.1715 + -1.6210i   vs  0.1715 + -1.6210i   -- error: 2.9649e-12
Ψ(1.7500) =  0.0275 + -1.7159i   vs  0.0275 + -1.7159i   -- error: 3.2760e-12
Ψ(1.8000) =  -0.1286 + -1.8002i  vs  -0.1286 + -1.8002i  -- error: 3.6031e-12
Ψ(1.8500) =  -0.2963 + -1.8721i  vs  -0.2963 + -1.8721i  -- error: 3.9449e-12
Ψ(1.9000) =  -0.4746 + -1.9298i  vs  -0.4746 + -1.9298i  -- error: 4.3044e-12
Ψ(1.9500) =  -0.6625 + -1.9713i  vs  -0.6625 + -1.9713i  -- error: 4.6861e-12
Ψ(2.0000) =  -0.8585 + -1.9950i  vs  -0.8585 + -1.9950i  -- error: 5.0851e-12
Ψ(2.0500) =  -1.0609 + -1.9991i  vs  -1.0609 + -1.9991i  -- error: 5.5040e-12
Ψ(2.1000) =  -1.2676 + -1.9820i  vs  -1.2676 + -1.9820i  -- error: 5.9395e-12
Ψ(2.1500) =  -1.4763 + -1.9425i  vs  -1.4763 + -1.9425i  -- error: 6.3965e-12
Ψ(2.2000) =  -1.6843 + -1.8793i  vs  -1.6843 + -1.8793i  -- error: 6.8736e-12
Ψ(2.2500) =  -1.8887 + -1.7917i  vs  -1.8887 + -1.7917i  -- error: 7.3742e-12
Ψ(2.3000) =  -2.0863 + -1.6793i  vs  -2.0863 + -1.6793i  -- error: 7.8985e-12
Ψ(2.3500) =  -2.2738 + -1.5419i  vs  -2.2738 + -1.5419i  -- error: 8.4415e-12
Ψ(2.4000) =  -2.4475 + -1.3801i  vs  -2.4475 + -1.3801i  -- error: 9.0049e-12
Ψ(2.4500) =  -2.6038 + -1.1949i  vs  -2.6038 + -1.1949i  -- error: 9.5983e-12
Ψ(2.5000) =  -2.7390 + -0.9878i  vs  -2.7390 + -0.9878i  -- error: 1.0214e-11
Ψ(2.5500) =  -2.8496 + -0.7610i  vs  -2.8496 + -0.7610i  -- error: 1.0855e-11
Ψ(2.6000) =  -2.9320 + -0.5172i  vs  -2.9320 + -0.5172i  -- error: 1.1521e-11
Ψ(2.6500) =  -2.9830 + -0.2599i  vs  -2.9830 + -0.2599i  -- error: 1.2208e-11
Ψ(2.7000) =  -3.0000 + 0.0068i   vs  -3.0000 + 0.0068i   -- error: 1.2925e-11
Ψ(2.7500) =  -2.9805 + 0.2784i   vs  -2.9805 + 0.2784i   -- error: 1.3666e-11
Ψ(2.8000) =  -2.9230 + 0.5496i   vs  -2.9230 + 0.5496i   -- error: 1.4435e-11
Ψ(2.8500) =  -2.8265 + 0.8149i   vs  -2.8265 + 0.8149i   -- error: 1.5231e-11
Ψ(2.9000) =  -2.6909 + 1.0681i   vs  -2.6909 + 1.0681i   -- error: 1.6063e-11
Ψ(2.9500) =  -2.5172 + 1.3031i   vs  -2.5172 + 1.3031i   -- error: 1.6918e-11
Ψ(3.0000) =  -2.3073 + 1.5136i   vs  -2.3073 + 1.5136i   -- error: 1.7803e-11
Ψ(3.0500) =  -2.0643 + 1.6933i   vs  -2.0643 + 1.6933i   -- error: 1.8717e-11
Ψ(3.1000) =  -1.7924 + 1.8363i   vs  -1.7924 + 1.8363i   -- error: 1.9664e-11
Ψ(3.1500) =  -1.4970 + 1.9373i   vs  -1.4970 + 1.9373i   -- error: 2.0642e-11
Ψ(3.2000) =  -1.1845 + 1.9915i   vs  -1.1845 + 1.9915i   -- error: 2.1650e-11
Ψ(3.2500) =  -0.8624 + 1.9953i   vs  -0.8624 + 1.9953i   -- error: 2.2693e-11
Ψ(3.3000) =  -0.5390 + 1.9461i   vs  -0.5390 + 1.9461i   -- error: 2.3761e-11
Ψ(3.3500) =  -0.2233 + 1.8430i   vs  -0.2233 + 1.8430i   -- error: 2.4865e-11
Ψ(3.4000) =   0.0752 + 1.6864i   vs   0.0752 + 1.6864i   -- error: 2.6004e-11
Ψ(3.4500) =   0.3469 + 1.4785i   vs   0.3469 + 1.4785i   -- error: 2.7176e-11
Ψ(3.5000) =   0.5822 + 1.2234i   vs   0.5822 + 1.2234i   -- error: 2.8382e-11
Ψ(3.5500) =   0.7722 + 0.9270i   vs   0.7722 + 0.9270i   -- error: 2.9623e-11
Ψ(3.6000) =   0.9088 + 0.5971i   vs   0.9088 + 0.5971i   -- error: 3.0899e-11
Ψ(3.6500) =   0.9852 + 0.2433i   vs   0.9852 + 0.2433i   -- error: 3.2211e-11
Ψ(3.7000) =  0.9962 + -0.1236i   vs  0.9962 + -0.1236i   -- error: 3.3560e-11
Ψ(3.7500) =  0.9388 + -0.4911i   vs  0.9388 + -0.4911i   -- error: 3.4946e-11
Ψ(3.8000) =  0.8122 + -0.8461i   vs  0.8122 + -0.8461i   -- error: 3.6368e-11
Ψ(3.8500) =  0.6183 + -1.1752i   vs  0.6183 + -1.1752i   -- error: 3.7831e-11
Ψ(3.9000) =  0.3618 + -1.4648i   vs  0.3618 + -1.4648i   -- error: 3.9333e-11
Ψ(3.9500) =  0.0500 + -1.7022i   vs  0.0500 + -1.7022i   -- error: 4.0870e-11
Ψ(4.0000) =  -0.3067 + -1.8760i  vs  -0.3067 + -1.8760i  -- error: 4.2450e-11
Ψ(4.0500) =  -0.6957 + -1.9767i  vs  -0.6957 + -1.9767i  -- error: 4.4068e-11
Ψ(4.1000) =  -1.1020 + -1.9974i  vs  -1.1020 + -1.9974i  -- error: 4.5725e-11
Ψ(4.1500) =  -1.5089 + -1.9342i  vs  -1.5089 + -1.9342i  -- error: 4.7425e-11
Ψ(4.2000) =  -1.8987 + -1.7867i  vs  -1.8987 + -1.7867i  -- error: 4.9166e-11
Ψ(4.2500) =  -2.2533 + -1.5586i  vs  -2.2533 + -1.5586i  -- error: 5.0951e-11
Ψ(4.3000) =  -2.5554 + -1.2572i  vs  -2.5554 + -1.2572i  -- error: 5.2779e-11
Ψ(4.3500) =  -2.7890 + -0.8942i  vs  -2.7890 + -0.8942i  -- error: 5.4646e-11
Ψ(4.4000) =  -2.9404 + -0.4847i  vs  -2.9404 + -0.4847i  -- error: 5.6560e-11
Ψ(4.4500) =  -2.9994 + -0.0471i  vs  -2.9994 + -0.0471i  -- error: 5.8509e-11
Ψ(4.5000) =  -2.9600 + 0.3978i   vs  -2.9600 + 0.3978i   -- error: 6.0505e-11
Ψ(4.5500) =  -2.8209 + 0.8273i   vs  -2.8209 + 0.8273i   -- error: 6.2553e-11
Ψ(4.6000) =  -2.5858 + 1.2187i   vs  -2.5858 + 1.2187i   -- error: 6.4641e-11
Ψ(4.6500) =  -2.2643 + 1.5497i   vs  -2.2643 + 1.5497i   -- error: 6.6781e-11
Ψ(4.7000) =  -1.8710 + 1.8004i   vs  -1.8710 + 1.8004i   -- error: 6.8970e-11
Ψ(4.7500) =  -1.4254 + 1.9542i   vs  -1.4254 + 1.9542i   -- error: 7.1194e-11
Ψ(4.8000) =  -0.9512 + 1.9994i   vs  -0.9512 + 1.9994i   -- error: 7.3478e-11
Ψ(4.8500) =  -0.4749 + 1.9298i   vs  -0.4749 + 1.9298i   -- error: 7.5802e-11
Ψ(4.9000) =  -0.0246 + 1.7460i   vs  -0.0246 + 1.7460i   -- error: 7.8180e-11
Ψ(4.9500) =   0.3716 + 1.4556i   vs   0.3716 + 1.4556i   -- error: 8.0607e-11
MAX ERROR: 8.0607e-11
ok

test result: ok. 1 passed; 0 failed; 0 ignored; 0 measured; 3 filtered out; finished in 0.16s
```
**Systems of Equations Solved:**


<img width="828" alt="Screenshot 2024-06-12 at 13 38 14" src="https://github.com/TomjWolcott/rust_fedvr_for_simple_ode/assets/134332655/d296b960-6bcf-43f1-a459-ddc569269450">


### Many DVRs back to back, with each initialized with the previous one
```
Ψ(______) =       Expected       vs       Computed      
Ψ(0.0000) =   0.7552 + 0.9589i   vs   0.7552 + 0.9589i   -- error: 2.1053e-12
Ψ(0.0500) =   0.7564 + 0.9567i   vs   0.7564 + 0.9567i   -- error: 2.0120e-12
Ψ(0.1000) =   0.7599 + 0.9501i   vs   0.7599 + 0.9501i   -- error: 1.9177e-12
Ψ(0.1500) =   0.7658 + 0.9390i   vs   0.7658 + 0.9390i   -- error: 1.8239e-12
Ψ(0.2000) =   0.7740 + 0.9236i   vs   0.7740 + 0.9236i   -- error: 1.7294e-12
Ψ(0.2500) =   0.7843 + 0.9035i   vs   0.7843 + 0.9035i   -- error: 1.6351e-12
Ψ(0.3000) =   0.7965 + 0.8789i   vs   0.7965 + 0.8789i   -- error: 1.5416e-12
Ψ(0.3500) =   0.8106 + 0.8496i   vs   0.8106 + 0.8496i   -- error: 1.4468e-12
Ψ(0.4000) =   0.8262 + 0.8155i   vs   0.8262 + 0.8155i   -- error: 1.3516e-12
Ψ(0.4500) =   0.8431 + 0.7765i   vs   0.8431 + 0.7765i   -- error: 1.2538e-12
Ψ(0.5000) =   0.8610 + 0.7325i   vs   0.8610 + 0.7325i   -- error: 1.1558e-12
Ψ(0.5500) =   0.8796 + 0.6834i   vs   0.8796 + 0.6834i   -- error: 1.0555e-12
Ψ(0.6000) =   0.8985 + 0.6291i   vs   0.8985 + 0.6291i   -- error: 9.5316e-13
Ψ(0.6500) =   0.9172 + 0.5695i   vs   0.9172 + 0.5695i   -- error: 8.4832e-13
Ψ(0.7000) =   0.9353 + 0.5045i   vs   0.9353 + 0.5045i   -- error: 7.4071e-13
Ψ(0.7500) =   0.9523 + 0.4340i   vs   0.9523 + 0.4340i   -- error: 6.2978e-13
Ψ(0.8000) =   0.9677 + 0.3581i   vs   0.9677 + 0.3581i   -- error: 5.1437e-13
Ψ(0.8500) =   0.9808 + 0.2766i   vs   0.9808 + 0.2766i   -- error: 3.9380e-13
Ψ(0.9000) =   0.9910 + 0.1897i   vs   0.9910 + 0.1897i   -- error: 2.6850e-13
Ψ(0.9500) =   0.9976 + 0.0975i   vs   0.9976 + 0.0975i   -- error: 1.3705e-13
Ψ(1.0000) =  1.0000 + -0.0000i   vs   1.0000 + 0.0000i   -- error: 9.3238e-17
Ψ(1.0500) =  0.9974 + -0.1025i   vs  0.9974 + -0.1025i   -- error: 1.4537e-13
Ψ(1.1000) =  0.9890 + -0.2096i   vs  0.9890 + -0.2096i   -- error: 2.9806e-13
Ψ(1.1500) =  0.9741 + -0.3211i   vs  0.9741 + -0.3211i   -- error: 4.5835e-13
Ψ(1.2000) =  0.9518 + -0.4365i   vs  0.9518 + -0.4365i   -- error: 6.2822e-13
Ψ(1.2500) =  0.9214 + -0.5551i   vs  0.9214 + -0.5551i   -- error: 8.0691e-13
Ψ(1.3000) =  0.8822 + -0.6764i   vs  0.8822 + -0.6764i   -- error: 9.9515e-13
Ψ(1.3500) =  0.8332 + -0.7995i   vs  0.8332 + -0.7995i   -- error: 1.1950e-12
Ψ(1.4000) =  0.7740 + -0.9236i   vs  0.7740 + -0.9236i   -- error: 1.4081e-12
Ψ(1.4500) =  0.7037 + -1.0475i   vs  0.7037 + -1.0475i   -- error: 1.6320e-12
Ψ(1.5000) =  0.6219 + -1.1702i   vs  0.6219 + -1.1702i   -- error: 1.8692e-12
Ψ(1.5500) =  0.5281 + -1.2903i   vs  0.5281 + -1.2903i   -- error: 2.1200e-12
Ψ(1.6000) =  0.4218 + -1.4066i   vs  0.4218 + -1.4066i   -- error: 2.3841e-12
Ψ(1.6500) =  0.3030 + -1.5173i   vs  0.3030 + -1.5173i   -- error: 2.6624e-12
Ψ(1.7000) =  0.1715 + -1.6210i   vs  0.1715 + -1.6210i   -- error: 2.9562e-12
Ψ(1.7500) =  0.0275 + -1.7159i   vs  0.0275 + -1.7159i   -- error: 3.2655e-12
Ψ(1.8000) =  -0.1286 + -1.8002i  vs  -0.1286 + -1.8002i  -- error: 3.5920e-12
Ψ(1.8500) =  -0.2963 + -1.8721i  vs  -0.2963 + -1.8721i  -- error: 3.9341e-12
Ψ(1.9000) =  -0.4746 + -1.9298i  vs  -0.4746 + -1.9298i  -- error: 4.2950e-12
Ψ(1.9500) =  -0.6625 + -1.9713i  vs  -0.6625 + -1.9713i  -- error: 4.6743e-12
Ψ(2.0000) =  -0.8585 + -1.9950i  vs  -0.8585 + -1.9950i  -- error: 5.0725e-12
Ψ(2.0500) =  -1.0609 + -1.9991i  vs  -1.0609 + -1.9991i  -- error: 5.4885e-12
Ψ(2.1000) =  -1.2676 + -1.9820i  vs  -1.2676 + -1.9820i  -- error: 5.9264e-12
Ψ(2.1500) =  -1.4763 + -1.9425i  vs  -1.4763 + -1.9425i  -- error: 6.3844e-12
Ψ(2.2000) =  -1.6843 + -1.8793i  vs  -1.6843 + -1.8793i  -- error: 6.8625e-12
Ψ(2.2500) =  -1.8887 + -1.7917i  vs  -1.8887 + -1.7917i  -- error: 7.3621e-12
Ψ(2.3000) =  -2.0863 + -1.6793i  vs  -2.0863 + -1.6793i  -- error: 7.8828e-12
Ψ(2.3500) =  -2.2738 + -1.5419i  vs  -2.2738 + -1.5419i  -- error: 8.4285e-12
Ψ(2.4000) =  -2.4475 + -1.3801i  vs  -2.4475 + -1.3801i  -- error: 8.9961e-12
Ψ(2.4500) =  -2.6038 + -1.1949i  vs  -2.6038 + -1.1949i  -- error: 9.5879e-12
Ψ(2.5000) =  -2.7390 + -0.9878i  vs  -2.7390 + -0.9878i  -- error: 1.0202e-11
Ψ(2.5500) =  -2.8496 + -0.7610i  vs  -2.8496 + -0.7610i  -- error: 1.0850e-11
Ψ(2.6000) =  -2.9320 + -0.5172i  vs  -2.9320 + -0.5172i  -- error: 1.1519e-11
Ψ(2.6500) =  -2.9830 + -0.2599i  vs  -2.9830 + -0.2599i  -- error: 1.2205e-11
Ψ(2.7000) =  -3.0000 + 0.0068i   vs  -3.0000 + 0.0068i   -- error: 1.2924e-11
Ψ(2.7500) =  -2.9805 + 0.2784i   vs  -2.9805 + 0.2784i   -- error: 1.3665e-11
Ψ(2.8000) =  -2.9230 + 0.5496i   vs  -2.9230 + 0.5496i   -- error: 1.4435e-11
Ψ(2.8500) =  -2.8265 + 0.8149i   vs  -2.8265 + 0.8149i   -- error: 1.5238e-11
Ψ(2.9000) =  -2.6909 + 1.0681i   vs  -2.6909 + 1.0681i   -- error: 1.6063e-11
Ψ(2.9500) =  -2.5172 + 1.3031i   vs  -2.5172 + 1.3031i   -- error: 1.6919e-11
Ψ(3.0000) =  -2.3073 + 1.5136i   vs  -2.3073 + 1.5136i   -- error: 1.7802e-11
Ψ(3.0500) =  -2.0643 + 1.6933i   vs  -2.0643 + 1.6933i   -- error: 1.8718e-11
Ψ(3.1000) =  -1.7924 + 1.8363i   vs  -1.7924 + 1.8363i   -- error: 1.9662e-11
Ψ(3.1500) =  -1.4970 + 1.9373i   vs  -1.4970 + 1.9373i   -- error: 2.0640e-11
Ψ(3.2000) =  -1.1845 + 1.9915i   vs  -1.1845 + 1.9915i   -- error: 2.1646e-11
Ψ(3.2500) =  -0.8624 + 1.9953i   vs  -0.8624 + 1.9953i   -- error: 2.2687e-11
Ψ(3.3000) =  -0.5390 + 1.9461i   vs  -0.5390 + 1.9461i   -- error: 2.3758e-11
Ψ(3.3500) =  -0.2233 + 1.8430i   vs  -0.2233 + 1.8430i   -- error: 2.4863e-11
Ψ(3.4000) =   0.0752 + 1.6864i   vs   0.0752 + 1.6864i   -- error: 2.6002e-11
Ψ(3.4500) =   0.3469 + 1.4785i   vs   0.3469 + 1.4785i   -- error: 2.7173e-11
Ψ(3.5000) =   0.5822 + 1.2234i   vs   0.5822 + 1.2234i   -- error: 2.8379e-11
Ψ(3.5500) =   0.7722 + 0.9270i   vs   0.7722 + 0.9270i   -- error: 2.9621e-11
Ψ(3.6000) =   0.9088 + 0.5971i   vs   0.9088 + 0.5971i   -- error: 3.0897e-11
Ψ(3.6500) =   0.9852 + 0.2433i   vs   0.9852 + 0.2433i   -- error: 3.2210e-11
Ψ(3.7000) =  0.9962 + -0.1236i   vs  0.9962 + -0.1236i   -- error: 3.3559e-11
Ψ(3.7500) =  0.9388 + -0.4911i   vs  0.9388 + -0.4911i   -- error: 3.4944e-11
Ψ(3.8000) =  0.8122 + -0.8461i   vs  0.8122 + -0.8461i   -- error: 3.6028e-11
Ψ(3.8500) =  0.6183 + -1.1752i   vs  0.6183 + -1.1752i   -- error: 3.7345e-11
Ψ(3.9000) =  0.3618 + -1.4648i   vs  0.3618 + -1.4648i   -- error: 3.8976e-11
Ψ(3.9500) =  0.0500 + -1.7022i   vs  0.0500 + -1.7022i   -- error: 4.0383e-11
Ψ(4.0000) =  -0.3067 + -1.8760i  vs  -0.3067 + -1.8760i  -- error: 4.2011e-11
Ψ(4.0500) =  -0.6957 + -1.9767i  vs  -0.6957 + -1.9767i  -- error: 4.3673e-11
Ψ(4.1000) =  -1.1020 + -1.9974i  vs  -1.1020 + -1.9974i  -- error: 4.5292e-11
Ψ(4.1500) =  -1.5089 + -1.9342i  vs  -1.5089 + -1.9342i  -- error: 4.6985e-11
Ψ(4.2000) =  -1.8987 + -1.7867i  vs  -1.8987 + -1.7867i  -- error: 4.8730e-11
Ψ(4.2500) =  -2.2533 + -1.5586i  vs  -2.2533 + -1.5586i  -- error: 5.0514e-11
Ψ(4.3000) =  -2.5554 + -1.2572i  vs  -2.5554 + -1.2572i  -- error: 5.2368e-11
Ψ(4.3500) =  -2.7890 + -0.8942i  vs  -2.7890 + -0.8942i  -- error: 5.4232e-11
Ψ(4.4000) =  -2.9404 + -0.4847i  vs  -2.9404 + -0.4847i  -- error: 5.6103e-11
Ψ(4.4500) =  -2.9994 + -0.0471i  vs  -2.9994 + -0.0471i  -- error: 5.8059e-11
Ψ(4.5000) =  -2.9600 + 0.3978i   vs  -2.9600 + 0.3978i   -- error: 6.0112e-11
Ψ(4.5500) =  -2.8209 + 0.8273i   vs  -2.8209 + 0.8273i   -- error: 6.2156e-11
Ψ(4.6000) =  -2.5858 + 1.2187i   vs  -2.5858 + 1.2187i   -- error: 6.4201e-11
Ψ(4.6500) =  -2.2643 + 1.5497i   vs  -2.2643 + 1.5497i   -- error: 6.6343e-11
Ψ(4.7000) =  -1.8710 + 1.8004i   vs  -1.8710 + 1.8004i   -- error: 6.8556e-11
Ψ(4.7500) =  -1.4254 + 1.9542i   vs  -1.4254 + 1.9542i   -- error: 7.0774e-11
Ψ(4.8000) =  -0.9512 + 1.9994i   vs  -0.9512 + 1.9994i   -- error: 7.3043e-11
Ψ(4.8500) =  -0.4749 + 1.9298i   vs  -0.4749 + 1.9298i   -- error: 7.5378e-11
Ψ(4.9000) =  -0.0246 + 1.7460i   vs  -0.0246 + 1.7460i   -- error: 7.7754e-11
Ψ(4.9500) =   0.3716 + 1.4556i   vs   0.3716 + 1.4556i   -- error: 8.0176e-11
MAX ERROR: 8.0176e-11
ok

test result: ok. 1 passed; 0 failed; 0 ignored; 0 measured; 3 filtered out; finished in 0.01s
```
**Systems of Equations Solved:**

<img width="248" alt="Screenshot 2024-06-12 at 13 41 59" src="https://github.com/TomjWolcott/rust_fedvr_for_simple_ode/assets/134332655/09747152-dade-4813-ba52-a74c0cd95a82">


### FEDVR using bridging functions to connect intervals
```
Ψ(______) =       Expected       vs       Computed      
Ψ(0.0000) =   0.7552 + 0.9589i   vs   0.7552 + 0.9589i   -- error: 8.8435e-11
Ψ(0.0500) =   0.7564 + 0.9567i   vs   0.7564 + 0.9567i   -- error: 1.9335e-11
Ψ(0.1000) =   0.7599 + 0.9501i   vs   0.7599 + 0.9501i   -- error: 1.1331e-11
Ψ(0.1500) =   0.7658 + 0.9390i   vs   0.7658 + 0.9390i   -- error: 1.8826e-11
Ψ(0.2000) =   0.7740 + 0.9236i   vs   0.7740 + 0.9236i   -- error: 1.5776e-11
Ψ(0.2500) =   0.7843 + 0.9035i   vs   0.7843 + 0.9035i   -- error: 2.0926e-12
Ψ(0.3000) =   0.7965 + 0.8789i   vs   0.7965 + 0.8789i   -- error: 1.7012e-11
Ψ(0.3500) =   0.8106 + 0.8496i   vs   0.8106 + 0.8496i   -- error: 8.0593e-12
Ψ(0.4000) =   0.8262 + 0.8155i   vs   0.8262 + 0.8155i   -- error: 1.2283e-11
Ψ(0.4500) =   0.8431 + 0.7765i   vs   0.8431 + 0.7765i   -- error: 1.1550e-11
Ψ(0.5000) =   0.8610 + 0.7325i   vs   0.8610 + 0.7325i   -- error: 1.1844e-11
Ψ(0.5500) =   0.8796 + 0.6834i   vs   0.8796 + 0.6834i   -- error: 1.1034e-11
Ψ(0.6000) =   0.8985 + 0.6291i   vs   0.8985 + 0.6291i   -- error: 1.0833e-11
Ψ(0.6500) =   0.9172 + 0.5695i   vs   0.9172 + 0.5695i   -- error: 1.1405e-11
Ψ(0.7000) =   0.9353 + 0.5045i   vs   0.9353 + 0.5045i   -- error: 1.1601e-11
Ψ(0.7500) =   0.9523 + 0.4340i   vs   0.9523 + 0.4340i   -- error: 1.1165e-11
Ψ(0.8000) =   0.9677 + 0.3581i   vs   0.9677 + 0.3581i   -- error: 1.0793e-11
Ψ(0.8500) =   0.9808 + 0.2766i   vs   0.9808 + 0.2766i   -- error: 1.2958e-11
Ψ(0.9000) =   0.9910 + 0.1897i   vs   0.9910 + 0.1897i   -- error: 8.7498e-12
Ψ(0.9500) =   0.9976 + 0.0975i   vs   0.9976 + 0.0975i   -- error: 1.5979e-11
Ψ(1.0000) =  1.0000 + -0.0000i   vs  1.0000 + -0.0000i   -- error: 4.6191e-16
Ψ(1.0500) =  0.9974 + -0.1025i   vs  0.9974 + -0.1025i   -- error: 1.6889e-11
Ψ(1.1000) =  0.9890 + -0.2096i   vs  0.9890 + -0.2096i   -- error: 1.7339e-11
Ψ(1.1500) =  0.9741 + -0.3211i   vs  0.9741 + -0.3211i   -- error: 1.2705e-11
Ψ(1.2000) =  0.9518 + -0.4365i   vs  0.9518 + -0.4365i   -- error: 1.7452e-11
Ψ(1.2500) =  0.9214 + -0.5551i   vs  0.9214 + -0.5551i   -- error: 8.6427e-11
Ψ(1.3000) =  0.8822 + -0.6764i   vs  0.8822 + -0.6764i   -- error: 3.2698e-11
Ψ(1.3500) =  0.8332 + -0.7995i   vs  0.8332 + -0.7995i   -- error: 1.3242e-11
Ψ(1.4000) =  0.7740 + -0.9236i   vs  0.7740 + -0.9236i   -- error: 3.1337e-11
Ψ(1.4500) =  0.7037 + -1.0475i   vs  0.7037 + -1.0475i   -- error: 1.4493e-11
Ψ(1.5000) =  0.6219 + -1.1702i   vs  0.6219 + -1.1702i   -- error: 1.8900e-11
Ψ(1.5500) =  0.5281 + -1.2903i   vs  0.5281 + -1.2903i   -- error: 2.8116e-11
Ψ(1.6000) =  0.4218 + -1.4066i   vs  0.4218 + -1.4066i   -- error: 1.7352e-11
Ψ(1.6500) =  0.3030 + -1.5173i   vs  0.3030 + -1.5173i   -- error: 1.7109e-11
Ψ(1.7000) =  0.1715 + -1.6210i   vs  0.1715 + -1.6210i   -- error: 2.1993e-11
Ψ(1.7500) =  0.0275 + -1.7159i   vs  0.0275 + -1.7159i   -- error: 2.2033e-11
Ψ(1.8000) =  -0.1286 + -1.8002i  vs  -0.1286 + -1.8002i  -- error: 1.9788e-11
Ψ(1.8500) =  -0.2963 + -1.8721i  vs  -0.2963 + -1.8721i  -- error: 1.9369e-11
Ψ(1.9000) =  -0.4746 + -1.9298i  vs  -0.4746 + -1.9298i  -- error: 1.7755e-11
Ψ(1.9500) =  -0.6625 + -1.9713i  vs  -0.6625 + -1.9713i  -- error: 1.7653e-11
Ψ(2.0000) =  -0.8585 + -1.9950i  vs  -0.8585 + -1.9950i  -- error: 2.1996e-11
Ψ(2.0500) =  -1.0609 + -1.9991i  vs  -1.0609 + -1.9991i  -- error: 2.1408e-11
Ψ(2.1000) =  -1.2676 + -1.9820i  vs  -1.2676 + -1.9820i  -- error: 1.2495e-11
Ψ(2.1500) =  -1.4763 + -1.9425i  vs  -1.4763 + -1.9425i  -- error: 1.2793e-11
Ψ(2.2000) =  -1.6843 + -1.8793i  vs  -1.6843 + -1.8793i  -- error: 2.6497e-11
Ψ(2.2500) =  -1.8887 + -1.7917i  vs  -1.8887 + -1.7917i  -- error: 1.4636e-11
Ψ(2.3000) =  -2.0863 + -1.6793i  vs  -2.0863 + -1.6793i  -- error: 5.9709e-12
Ψ(2.3500) =  -2.2738 + -1.5419i  vs  -2.2738 + -1.5419i  -- error: 2.7852e-11
Ψ(2.4000) =  -2.4475 + -1.3801i  vs  -2.4475 + -1.3801i  -- error: 4.0612e-12
Ψ(2.4500) =  -2.6038 + -1.1949i  vs  -2.6038 + -1.1949i  -- error: 2.7416e-11
Ψ(2.5000) =  -2.7390 + -0.9878i  vs  -2.7390 + -0.9878i  -- error: 8.1023e-11
Ψ(2.5500) =  -2.8496 + -0.7610i  vs  -2.8496 + -0.7610i  -- error: 6.9946e-11
Ψ(2.6000) =  -2.9320 + -0.5172i  vs  -2.9320 + -0.5172i  -- error: 6.4485e-11
Ψ(2.6500) =  -2.9830 + -0.2599i  vs  -2.9830 + -0.2599i  -- error: 6.9358e-11
Ψ(2.7000) =  -3.0000 + 0.0068i   vs  -3.0000 + 0.0068i   -- error: 6.1669e-11
Ψ(2.7500) =  -2.9805 + 0.2784i   vs  -2.9805 + 0.2784i   -- error: 6.4662e-11
Ψ(2.8000) =  -2.9230 + 0.5496i   vs  -2.9230 + 0.5496i   -- error: 6.7514e-11
Ψ(2.8500) =  -2.8265 + 0.8149i   vs  -2.8265 + 0.8149i   -- error: 6.1142e-11
Ψ(2.9000) =  -2.6909 + 1.0681i   vs  -2.6909 + 1.0681i   -- error: 5.9396e-11
Ψ(2.9500) =  -2.5172 + 1.3031i   vs  -2.5172 + 1.3031i   -- error: 6.3798e-11
Ψ(3.0000) =  -2.3073 + 1.5136i   vs  -2.3073 + 1.5136i   -- error: 6.3097e-11
Ψ(3.0500) =  -2.0643 + 1.6933i   vs  -2.0643 + 1.6933i   -- error: 5.8080e-11
Ψ(3.1000) =  -1.7924 + 1.8363i   vs  -1.7924 + 1.8363i   -- error: 5.7176e-11
Ψ(3.1500) =  -1.4970 + 1.9373i   vs  -1.4970 + 1.9373i   -- error: 5.9222e-11
Ψ(3.2000) =  -1.1845 + 1.9915i   vs  -1.1845 + 1.9915i   -- error: 5.8409e-11
Ψ(3.2500) =  -0.8624 + 1.9953i   vs  -0.8624 + 1.9953i   -- error: 5.6119e-11
Ψ(3.3000) =  -0.5390 + 1.9461i   vs  -0.5390 + 1.9461i   -- error: 5.5042e-11
Ψ(3.3500) =  -0.2233 + 1.8430i   vs  -0.2233 + 1.8430i   -- error: 5.3549e-11
Ψ(3.4000) =   0.0752 + 1.6864i   vs   0.0752 + 1.6864i   -- error: 5.2871e-11
Ψ(3.4500) =   0.3469 + 1.4785i   vs   0.3469 + 1.4785i   -- error: 5.4374e-11
Ψ(3.5000) =   0.5822 + 1.2234i   vs   0.5822 + 1.2234i   -- error: 5.0856e-11
Ψ(3.5500) =   0.7722 + 0.9270i   vs   0.7722 + 0.9270i   -- error: 4.7013e-11
Ψ(3.6000) =   0.9088 + 0.5971i   vs   0.9088 + 0.5971i   -- error: 5.2676e-11
Ψ(3.6500) =   0.9852 + 0.2433i   vs   0.9852 + 0.2433i   -- error: 4.4891e-11
Ψ(3.7000) =  0.9962 + -0.1236i   vs  0.9962 + -0.1236i   -- error: 5.0658e-11
Ψ(3.7500) =  0.9388 + -0.4911i   vs  0.9388 + -0.4911i   -- error: 6.4987e-11
Ψ(3.8000) =  0.8122 + -0.8461i   vs  0.8122 + -0.8461i   -- error: 6.4032e-11
Ψ(3.8500) =  0.6183 + -1.1752i   vs  0.6183 + -1.1752i   -- error: 6.2845e-11
Ψ(3.9000) =  0.3618 + -1.4648i   vs  0.3618 + -1.4648i   -- error: 6.1416e-11
Ψ(3.9500) =  0.0500 + -1.7022i   vs  0.0500 + -1.7022i   -- error: 6.0123e-11
Ψ(4.0000) =  -0.3067 + -1.8760i  vs  -0.3067 + -1.8760i  -- error: 5.8717e-11
Ψ(4.0500) =  -0.6957 + -1.9767i  vs  -0.6957 + -1.9767i  -- error: 5.7292e-11
Ψ(4.1000) =  -1.1020 + -1.9974i  vs  -1.1020 + -1.9974i  -- error: 5.5835e-11
Ψ(4.1500) =  -1.5089 + -1.9342i  vs  -1.5089 + -1.9342i  -- error: 5.4365e-11
Ψ(4.2000) =  -1.8987 + -1.7867i  vs  -1.8987 + -1.7867i  -- error: 5.2923e-11
Ψ(4.2500) =  -2.2533 + -1.5586i  vs  -2.2533 + -1.5586i  -- error: 5.1417e-11
Ψ(4.3000) =  -2.5554 + -1.2572i  vs  -2.5554 + -1.2572i  -- error: 4.9830e-11
Ψ(4.3500) =  -2.7890 + -0.8942i  vs  -2.7890 + -0.8942i  -- error: 4.8291e-11
Ψ(4.4000) =  -2.9404 + -0.4847i  vs  -2.9404 + -0.4847i  -- error: 4.6798e-11
Ψ(4.4500) =  -2.9994 + -0.0471i  vs  -2.9994 + -0.0471i  -- error: 4.5239e-11
Ψ(4.5000) =  -2.9600 + 0.3978i   vs  -2.9600 + 0.3978i   -- error: 4.3625e-11
Ψ(4.5500) =  -2.8209 + 0.8273i   vs  -2.8209 + 0.8273i   -- error: 4.2064e-11
Ψ(4.6000) =  -2.5858 + 1.2187i   vs  -2.5858 + 1.2187i   -- error: 4.0538e-11
Ψ(4.6500) =  -2.2643 + 1.5497i   vs  -2.2643 + 1.5497i   -- error: 3.9001e-11
Ψ(4.7000) =  -1.8710 + 1.8004i   vs  -1.8710 + 1.8004i   -- error: 3.7489e-11
Ψ(4.7500) =  -1.4254 + 1.9542i   vs  -1.4254 + 1.9542i   -- error: 3.6013e-11
Ψ(4.8000) =  -0.9512 + 1.9994i   vs  -0.9512 + 1.9994i   -- error: 3.4584e-11
Ψ(4.8500) =  -0.4749 + 1.9298i   vs  -0.4749 + 1.9298i   -- error: 3.3250e-11
Ψ(4.9000) =  -0.0246 + 1.7460i   vs  -0.0246 + 1.7460i   -- error: 3.1972e-11
Ψ(4.9500) =   0.3716 + 1.4556i   vs   0.3716 + 1.4556i   -- error: 3.0820e-11
MAX ERROR: 8.8435e-11
ok
test test_solving_ode ...
```
**Systems of Equations Solved:**


<img width="805" alt="Screenshot 2024-06-12 at 13 38 53" src="https://github.com/TomjWolcott/rust_fedvr_for_simple_ode/assets/134332655/f9954912-f18d-47f0-a66f-378a711702ff">
