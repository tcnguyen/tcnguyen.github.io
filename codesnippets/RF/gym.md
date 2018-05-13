---
layout: page
title: Gym
---

### Record video

```python
from gym import wrappers
env = gym.make('CartPole-v0')
env = wrappers.Monitor(env, './videos')
env.reset()
while True:
        new_state, reward, done, _ = env.step(env.action_space.sample())
        if done:
            break
```