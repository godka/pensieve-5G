# Pensieve PPO for 5G traces

Key Motivation

I further investigate the key reason why RL-based methods fail to perform well in 5G. Here are some key findings.

1.  the state inputs have not been accurately normalized with the proper scale, while recent work illustrates that state normalization is critical for RL-based tasks [1].
For example, state[2] represents the throughput in Mbps, while most of the bandwidth in 5G is over 100Mbps, even 1Gbps, and eventually, resulting in the unexcepted scale. Some observations are also shown in the video chunk sizes.

https://github.com/SIGCOMM21-5G/artifact/blob/df62dc3bb8661eb2bdca4f853b6dd2d396b28007/Video-Streaming/ABR-5G/rl_server/rl_server_no_training.py#L150

Some observations are also shown in the video chunk sizes (MB).

https://github.com/SIGCOMM21-5G/artifact/blob/df62dc3bb8661eb2bdca4f853b6dd2d396b28007/Video-Streaming/ABR-5G/rl_server/rl_server_no_training.py#L152

Meanwhile, the reward function requires reward scaling since the maximum bitrate is sized over 160Mbps.

2. Another feature is inherited from Pensieve. The download time measured by the RL server means **chunk download end - chunk download start**. However, in the simulator, the download time is computed as **chunk ended - request start**. Thus, there exists a gap between those two, i.e., an RTT. Note that such gaps are acceptable for 3G and 4G traces.

https://github.com/SIGCOMM21-5G/artifact/blob/df62dc3bb8661eb2bdca4f853b6dd2d396b28007/Video-Streaming/ABR-5G/rl_server/rl_server_no_training.py#L132 

[Pensieve simulator: delay](https://github.com/hongzimao/pensieve/blob/1120bb173958dc9bc9f2ebff1a8fe688b6f4e93c/sim/env.py#L89)

Putting them together, we retrained Pensieve w.r.t 5G datasets and videos [godka/Pensieve-5G](https://github.com/godka/pensieve-5G).

However, due to the coronavirus outbreak, we are not allowed to work in the lab. Thus, we employed trace-driven simulation rather than emulation. We first confirmed the correctness of the simulation environment. We compared the results of simulation and emulation via the pre-trained Pensieve model (i.e., nn_model_44200.ckpt). Origin Emulation Results vs. Our Simulation Results are shown below.

![Origin Emulation Results](https://user-images.githubusercontent.com/13187954/130193194-aaf93b48-c8ca-45c9-a77b-f64087dde93c.png "Origin Emulation Results")

![Our Simulation Results](https://user-images.githubusercontent.com/13187954/130193238-a97fbb32-0390-456f-923c-c8c95752ee78.png "Our Simulation Results")

Next, we reported our retrained results here. As shown, we can see that Pensieve can also achieve a good performance in 5G networks.

![figure17-retrained](https://user-images.githubusercontent.com/13187954/130193589-7095165f-b5af-429b-a5ce-5a41aac857f0.png)

Moreover, we demonstrated Pensieve's last 50,000 epochs, aka., Pensieve's Pareto Frontier. As expected, Pensieve can always satisfy the requirements.

![figure17-PF](https://user-images.githubusercontent.com/13187954/130193762-9867cf71-3a35-4673-baf1-1ddad535bddc.png)

_In general, we argue that Pensieve can also be well-performed in 5G networks._ The key reason for the results in the paper is the lack of RL tricks, such as state normalization and sim-to-real gaps, rather than Pensieve itself. 
At the same time, we also provide the pre-trained model in [here](https://github.com/godka/pensieve-5G/tree/master/src/results).
Moreover, another team run the model on our emulation setup and get similar results as we posted (plot attached).
![130872647-7d0fbe09-3ab6-47d1-b59f-6ccd9aa12ac2](https://user-images.githubusercontent.com/13187954/130906093-a48693a8-f2f4-4e93-8067-610532ffaff1.png)


Reference
[1] Abbasloo S, Yen C Y, Chao H J. Classic meets modern: A pragmatic learning-based congestion control for the Internet[C]//Proceedings of the Annual conference of the ACM Special Interest Group on Data Communication on the applications, technologies, architectures, and protocols for computer communication. 2020: 632-647.
