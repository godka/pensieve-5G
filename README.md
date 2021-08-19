# Pensieve PPO for 5G traces

## About Pensive-PPO 5G

This is an easy TensorFlow implementation of Pensieve[1]. 
In detail, we trained Pensieve via PPO rather than A3C.
It's a stable version, which has already prepared the training set and the test set, and you can run the repo easily: just type

```
python train.py
```

instead. Results will be evaluated on the test set (from 5G) every 300 epochs.


