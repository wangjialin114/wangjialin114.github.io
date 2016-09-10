## tensorflow Error-List

1. AttributeError: 'LSTMStateTuple' object has no attribute 'eval'

**sol**:

错误代码：

```
state = m._initial_state.eval()
```

修改后：

```
state = session.run(m._initial_state)
```
2. modprobe: FATAL: Module nvidia-uvm not found in directory /lib/modules/4.4.0-36-generic

**sol**:
重新安装新的nvidia driver

```
sudo aptitude reinstall nvidia-361
```