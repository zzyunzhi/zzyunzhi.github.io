# Position Encoding functions in Transformer Networks
Ashish Vaswani, Noam Shazeer, Niki Parmar, Jakob Uszkoreit, Llion Jones, Aidan N. Gomez, Lukasz Kaiser, Illia Polosukhin. Attention Is All You Need. 2017. 

## Previous Work
- Recurrent models: the sequential processing of input symbols precludes parallelization, and runtime scales up with longer sequence lengths. 
- Attention mechanisms: allows dependency without regard to distance, used with recurrence.  
- Transformer network: allows for parallelization, removing recurrence and convolution while maintaining global dependencies. 

## Discussion on Position Encoding
Authors of the paper comment on the choice of position encoding function with "for any fixed offset k, PE(pos+k) can be represented as a linear function of PE(pos)". Combined with illustration by Jay Alammar, the transformation is linear in the sense that it is a row rotation of the encoded position vector, i.e. f(pos+k, j) = f(pos, j-delta) where delta is independent of j when k << pos. 
The following is the proof.
![proof](./proof.jpg "proof")

