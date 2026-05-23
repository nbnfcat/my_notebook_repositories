```markdown
# LLaMA.cpp HTTP 服务器

基于 [httplib](https://github.com/yhirose/cpp-httplib)、[nlohmann::json](https://github.com/nlohmann/json) 和 **llama.cpp** 的轻量级纯 C/C++ HTTP 服务器。

提供一组 LLM REST API 和 Web UI，用于与 llama.cpp 交互。

**特性：**
* 在 GPU 和 CPU 上推理 F16 和量化模型
* 兼容 [OpenAI API](https://github.com/openai/openai-openapi) 的聊天补全、响应和嵌入路由
* 兼容 [Anthropic Messages API](https://docs.anthropic.com/en/api/messages) 的聊天补全
* 重排序端点 (https://github.com/ggml-org/llama.cpp/pull/9510)
* 支持多用户的并行解码
* 连续批处理
* 多模态 ([文档](../../docs/multimodal.md)) / 兼容 OpenAI API
* 监控端点
* 受 JSON Schema 约束的响应格式
* 类似 Claude API 的助手消息预填充
* [函数调用](../../docs/function-calling.md) / 适用于几乎任何模型的工具使用
* 推测解码
* 易于使用的 Web UI

有关完整功能列表，请参阅[服务器更新日志](https://github.com/ggml-org/llama.cpp/issues/9291)

## 用法

<!-- HELP_START -->

<!-- 重要提示：以下列表由 llama-gen-docs 自动生成；请勿手动修改 -->

### 通用参数

| 参数 | 说明 |
| ---- | ---- |
| `-h, --help, --usage` | 打印用法信息并退出 |
| `--version` | 显示版本和构建信息 |
| `--license` | 显示源代码许可证和依赖项 |
| `-cl, --cache-list` | 显示缓存中的模型列表 |
| `--completion-bash` | 打印可 source 的 llama.cpp bash 补全脚本 |
| `-t, --threads N` | 生成时使用的 CPU 线程数（默认：-1）<br/>（环境变量：LLAMA_ARG_THREADS） |
| `-tb, --threads-batch N` | 批处理和提示处理时使用的线程数（默认：同 --threads） |
| `-C, --cpu-mask M` | CPU 亲和性掩码：任意长度的十六进制数，与 cpu-range 互补（默认：""） |
| `-Cr, --cpu-range lo-hi` | 用于亲和性设置的 CPU 范围，与 --cpu-mask 互补 |
| `--cpu-strict <0\|1>` | 使用严格的 CPU 放置（默认：0） |
| `--prio N` | 设置进程/线程优先级：low(-1), normal(0), medium(1), high(2), realtime(3)（默认：0） |
| `--poll <0...100>` | 使用轮询等待工作（0 - 无轮询，默认：50） |
| `-Cb, --cpu-mask-batch M` | CPU 亲和性掩码：任意长度的十六进制数，与 cpu-range-batch 互补（默认：同 --cpu-mask） |
| `-Crb, --cpu-range-batch lo-hi` | 用于亲和性设置的 CPU 范围，与 --cpu-mask-batch 互补 |
| `--cpu-strict-batch <0\|1>` | 使用严格的 CPU 放置（默认：同 --cpu-strict） |
| `--prio-batch N` | 设置进程/线程优先级：0-normal, 1-medium, 2-high, 3-realtime（默认：0） |
| `--poll-batch <0\|1>` | 使用轮询等待工作（默认：同 --poll） |
| `-c, --ctx-size N` | 提示上下文大小（默认：0，0 = 从模型加载）<br/>（环境变量：LLAMA_ARG_CTX_SIZE） |
| `-n, --predict, --n-predict N` | 预测的 token 数量（默认：-1，-1 = 无限）<br/>（环境变量：LLAMA_ARG_N_PREDICT） |
| `-b, --batch-size N` | 逻辑最大批量大小（默认：2048）<br/>（环境变量：LLAMA_ARG_BATCH） |
| `-ub, --ubatch-size N` | 物理最大批量大小（默认：512）<br/>（环境变量：LLAMA_ARG_UBATCH） |
| `--keep N` | 从初始提示中保留的 token 数量（默认：0，-1 = 全部） |
| `--swa-full` | 使用完整大小的 SWA 缓存（默认：false）<br/>[更多信息](https://github.com/ggml-org/llama.cpp/pull/13194#issuecomment-2868343055)<br/>（环境变量：LLAMA_ARG_SWA_FULL） |
| `-fa, --flash-attn [on\|off\|auto]` | 设置 Flash Attention 使用（'on', 'off', 或 'auto'，默认：'auto'）<br/>（环境变量：LLAMA_ARG_FLASH_ATTN） |
| `--perf, --no-perf` | 是否启用内部 libllama 性能计时（默认：false）<br/>（环境变量：LLAMA_ARG_PERF） |
| `-e, --escape, --no-escape` | 是否处理转义序列（\n, \r, \t, \', \", \\）（默认：true） |
| `--rope-scaling {none,linear,yarn}` | RoPE 频率缩放方法，除非模型指定，否则默认为 linear<br/>（环境变量：LLAMA_ARG_ROPE_SCALING_TYPE） |
| `--rope-scale N` | RoPE 上下文缩放因子，将上下文扩大 N 倍<br/>（环境变量：LLAMA_ARG_ROPE_SCALE） |
| `--rope-freq-base N` | RoPE 基频，用于 NTK 感知缩放（默认：从模型加载）<br/>（环境变量：LLAMA_ARG_ROPE_FREQ_BASE） |
| `--rope-freq-scale N` | RoPE 频率缩放因子，将上下文扩大 1/N 倍<br/>（环境变量：LLAMA_ARG_ROPE_FREQ_SCALE） |
| `--yarn-orig-ctx N` | YaRN：模型的原始上下文大小（默认：0 = 模型训练上下文大小）<br/>（环境变量：LLAMA_ARG_YARN_ORIG_CTX） |
| `--yarn-ext-factor N` | YaRN：外推混合因子（默认：-1.00，0.0 = 完全内插）<br/>（环境变量：LLAMA_ARG_YARN_EXT_FACTOR） |
| `--yarn-attn-factor N` | YaRN：缩放 sqrt(t) 或注意力幅度（默认：-1.00）<br/>（环境变量：LLAMA_ARG_YARN_ATTN_FACTOR） |
| `--yarn-beta-slow N` | YaRN：高修正维度或 alpha（默认：-1.00）<br/>（环境变量：LLAMA_ARG_YARN_BETA_SLOW） |
| `--yarn-beta-fast N` | YaRN：低修正维度或 beta（默认：-1.00）<br/>（环境变量：LLAMA_ARG_YARN_BETA_FAST） |
| `-kvo, --kv-offload, -nkvo, --no-kv-offload` | 是否启用 KV 缓存卸载（默认：启用）<br/>（环境变量：LLAMA_ARG_KV_OFFLOAD） |
| `--repack, -nr, --no-repack` | 是否启用权重重打包（默认：启用）<br/>（环境变量：LLAMA_ARG_REPACK） |
| `--no-host` | 绕过主机缓冲区，允许使用额外缓冲区<br/>（环境变量：LLAMA_ARG_NO_HOST） |
| `-ctk, --cache-type-k TYPE` | K 的 KV 缓存数据类型<br/>允许的值：f32, f16, bf16, q8_0, q4_0, q4_1, iq4_nl, q5_0, q5_1<br/>（默认：f16）<br/>（环境变量：LLAMA_ARG_CACHE_TYPE_K） |
| `-ctv, --cache-type-v TYPE` | V 的 KV 缓存数据类型<br/>允许的值：f32, f16, bf16, q8_0, q4_0, q4_1, iq4_nl, q5_0, q5_1<br/>（默认：f16）<br/>（环境变量：LLAMA_ARG_CACHE_TYPE_V） |
| `-dt, --defrag-thold N` | KV 缓存碎片整理阈值（已弃用）<br/>（环境变量：LLAMA_ARG_DEFRAG_THOLD） |
| `--mlock` | 强制系统将模型保留在 RAM 中，而不是交换或压缩<br/>（环境变量：LLAMA_ARG_MLOCK） |
| `--mmap, --no-mmap` | 是否内存映射模型（如果禁用 mmap，加载更慢但如果不使用 mlock 可能减少页面交换）（默认：启用）<br/>（环境变量：LLAMA_ARG_MMAP） |
| `-dio, --direct-io, -ndio, --no-direct-io` | 如果可用则使用 DirectIO（默认：禁用）<br/>（环境变量：LLAMA_ARG_DIO） |
| `--numa TYPE` | 尝试优化，有助于某些 NUMA 系统<br/>- distribute: 在所有节点上均匀分布执行<br/>- isolate: 仅在执行开始所在节点上的 CPU 上生成线程<br/>- numactl: 使用 numactl 提供的 CPU 映射<br/>如果之前没有使用此选项运行，建议在使用前清除系统页缓存<br/>参见 https://github.com/ggml-org/llama.cpp/issues/1437<br/>（环境变量：LLAMA_ARG_NUMA） |
| `-dev, --device <dev1,dev2,..>` | 用于卸载的设备列表，逗号分隔（none = 不卸载）<br/>使用 --list-devices 查看可用设备列表<br/>（环境变量：LLAMA_ARG_DEVICE） |
| `--list-devices` | 打印可用设备列表并退出 |
| `-ot, --override-tensor <张量名模式>=<缓冲区类型>,...` | 覆盖张量缓冲区类型<br/>（环境变量：LLAMA_ARG_OVERRIDE_TENSOR） |
| `-cmoe, --cpu-moe` | 将所有混合专家（MoE）权重保留在 CPU 上<br/>（环境变量：LLAMA_ARG_CPU_MOE） |
| `-ncmoe, --n-cpu-moe N` | 将前 N 层的混合专家（MoE）权重保留在 CPU 上<br/>（环境变量：LLAMA_ARG_N_CPU_MOE） |
| `-ngl, --gpu-layers, --n-gpu-layers N` | 存储在 VRAM 中的最大层数，可以是具体数字、'auto' 或 'all'（默认：auto）<br/>（环境变量：LLAMA_ARG_N_GPU_LAYERS） |
| `-sm, --split-mode {none,layer,row}` | 如何在多个 GPU 间拆分模型，可选值：<br/>- none: 仅使用一个 GPU<br/>- layer（默认）: 跨 GPU 拆分层和 KV<br/>- row: 跨 GPU 拆分行<br/>（环境变量：LLAMA_ARG_SPLIT_MODE） |
| `-ts, --tensor-split N0,N1,N2,...` | 每个 GPU 卸载模型的分数，逗号分隔的比例列表，例如 3,1<br/>（环境变量：LLAMA_ARG_TENSOR_SPLIT） |
| `-mg, --main-gpu INDEX` | 用于模型的 GPU（split-mode = none 时），或用于中间结果和 KV（split-mode = row 时）（默认：0）<br/>（环境变量：LLAMA_ARG_MAIN_GPU） |
| `-fit, --fit [on\|off]` | 是否调整未设置的参数以适应设备内存（'on' 或 'off'，默认：'on'）<br/>（环境变量：LLAMA_ARG_FIT） |
| `-fitt, --fit-target MiB0,MiB1,MiB2,...` | 每个设备的 --fit 目标余量（MiB），逗号分隔列表，单个值广播到所有设备，默认：1024<br/>（环境变量：LLAMA_ARG_FIT_TARGET） |
| `-fitc, --fit-ctx N` | --fit 选项可设置的最小 ctx 大小，默认：4096<br/>（环境变量：LLAMA_ARG_FIT_CTX） |
| `--check-tensors` | 检查模型张量数据是否存在无效值（默认：false） |
| `--override-kv KEY=TYPE:VALUE,...` | 高级选项：按键覆盖模型元数据。要指定多个覆盖，请使用逗号分隔的值。<br/>类型：int, float, bool, str。示例：--override-kv tokenizer.ggml.add_bos_token=bool:false,tokenizer.ggml.add_eos_token=bool:false |
| `--op-offload, --no-op-offload` | 是否将主机张量操作卸载到设备（默认：true） |
| `--lora FNAME` | LoRA 适配器路径（使用逗号分隔值加载多个适配器） |
| `--lora-scaled FNAME:SCALE,...` | 带用户定义缩放的 LoRA 适配器路径（格式：FNAME:SCALE,...）<br/>注意：使用逗号分隔值 |
| `--control-vector FNAME` | 添加控制向量<br/>注意：使用逗号分隔值添加多个控制向量 |
| `--control-vector-scaled FNAME:SCALE,...` | 添加带用户定义缩放 SCALE 的控制向量<br/>注意：使用逗号分隔值（格式：FNAME:SCALE,...） |
| `--control-vector-layer-range START END` | 应用控制向量的层范围，起始和结束包含 |
| `-m, --model FNAME` | 模型路径<br/>（环境变量：LLAMA_ARG_MODEL） |
| `-mu, --model-url MODEL_URL` | 模型下载 URL（默认：未使用）<br/>（环境变量：LLAMA_ARG_MODEL_URL） |
| `-dr, --docker-repo [<仓库>/]<模型>[:量化]` | Docker Hub 模型仓库。仓库可选，默认为 ai/。量化可选，默认为 :latest。<br/>示例：gemma3<br/>（默认：未使用）<br/>（环境变量：LLAMA_ARG_DOCKER_REPO） |
| `-hf, -hfr, --hf-repo <用户>/<模型>[:量化]` | Hugging Face 模型仓库；量化可选，不区分大小写，默认为 Q4_K_M，如果仓库中没有 Q4_K_M 则回退到第一个文件。<br/>如果可用，也会自动下载 mmproj。要禁用，添加 --no-mmproj<br/>示例：ggml-org/GLM-4.7-Flash-GGUF:Q4_K_M<br/>（默认：未使用）<br/>（环境变量：LLAMA_ARG_HF_REPO） |
| `-hfd, -hfrd, --hf-repo-draft <用户>/<模型>[:量化]` | 与 --hf-repo 相同，但用于草稿模型（默认：未使用）<br/>（环境变量：LLAMA_ARG_HFD_REPO） |
| `-hff, --hf-file FILE` | Hugging Face 模型文件。如果指定，将覆盖 --hf-repo 中的量化（默认：未使用）<br/>（环境变量：LLAMA_ARG_HF_FILE） |
| `-hfv, -hfrv, --hf-repo-v <用户>/<模型>[:量化]` | 声码器模型的 Hugging Face 模型仓库（默认：未使用）<br/>（环境变量：LLAMA_ARG_HF_REPO_V） |
| `-hffv, --hf-file-v FILE` | 声码器模型的 Hugging Face 模型文件（默认：未使用）<br/>（环境变量：LLAMA_ARG_HF_FILE_V） |
| `-hft, --hf-token TOKEN` | Hugging Face 访问令牌（默认：来自 HF_TOKEN 环境变量的值）<br/>（环境变量：HF_TOKEN） |
| `--log-disable` | 禁用日志 |
| `--log-file FNAME` | 日志输出到文件<br/>（环境变量：LLAMA_LOG_FILE） |
| `--log-colors [on\|off\|auto]` | 设置彩色日志（'on', 'off', 或 'auto'，默认：'auto'）<br/>'auto' 当输出到终端时启用颜色<br/>（环境变量：LLAMA_LOG_COLORS） |
| `-v, --verbose, --log-verbose` | 将详细级别设置为无限（即记录所有消息，用于调试） |
| `--offline` | 离线模式：强制使用缓存，阻止网络访问<br/>（环境变量：LLAMA_OFFLINE） |
| `-lv, --verbosity, --log-verbosity N` | 设置详细级别阈值。详细程度更高的消息将被忽略。值：<br/> - 0: 通用输出<br/> - 1: 错误<br/> - 2: 警告<br/> - 3: 信息<br/> - 4: 调试<br/>（默认：3）<br/><br/>（环境变量：LLAMA_LOG_VERBOSITY） |
| `--log-prefix` | 在日志消息中启用前缀<br/>（环境变量：LLAMA_LOG_PREFIX） |
| `--log-timestamps` | 在日志消息中启用时间戳<br/>（环境变量：LLAMA_LOG_TIMESTAMPS） |
| `-ctkd, --cache-type-k-draft TYPE` | 草稿模型 K 的 KV 缓存数据类型<br/>允许的值：f32, f16, bf16, q8_0, q4_0, q4_1, iq4_nl, q5_0, q5_1<br/>（默认：f16）<br/>（环境变量：LLAMA_ARG_CACHE_TYPE_K_DRAFT） |
| `-ctvd, --cache-type-v-draft TYPE` | 草稿模型 V 的 KV 缓存数据类型<br/>允许的值：f32, f16, bf16, q8_0, q4_0, q4_1, iq4_nl, q5_0, q5_1<br/>（默认：f16）<br/>（环境变量：LLAMA_ARG_CACHE_TYPE_V_DRAFT） |

### 采样参数

| 参数 | 说明 |
| ---- | ---- |
| `--samplers SAMPLERS` | 将按顺序使用的采样器，以 ';' 分隔<br/>（默认：penalties;dry;top_n_sigma;top_k;typ_p;top_p;min_p;xtc;temperature） |
| `-s, --seed SEED` | RNG 种子（默认：-1，-1 使用随机种子） |
| `--sampler-seq, --sampling-seq SEQUENCE` | 简化采样器序列（默认：edskypmxt） |
| `--ignore-eos` | 忽略流结束 token 并继续生成（隐含 --logit-bias EOS-inf） |
| `--temp, --temperature N` | 温度（默认：0.80） |
| `--top-k N` | top-k 采样（默认：40，0 = 禁用）<br/>（环境变量：LLAMA_ARG_TOP_K） |
| `--top-p N` | top-p 采样（默认：0.95，1.0 = 禁用） |
| `--min-p N` | min-p 采样（默认：0.05，0.0 = 禁用） |
| `--top-nsigma, --top-n-sigma N` | top-n-sigma 采样（默认：-1.00，-1.0 = 禁用） |
| `--xtc-probability N` | xtc 概率（默认：0.00，0.0 = 禁用） |
| `--xtc-threshold N` | xtc 阈值（默认：0.10，1.0 = 禁用） |
| `--typical, --typical-p N` | 局部典型采样，参数 p（默认：1.00，1.0 = 禁用） |
| `--repeat-last-n N` | 最后 n 个 token 用于惩罚（默认：64，0 = 禁用，-1 = ctx_size） |
| `--repeat-penalty N` | 惩罚重复序列 token（默认：1.00，1.0 = 禁用） |
| `--presence-penalty N` | 存在惩罚（默认：0.00，0.0 = 禁用） |
| `--frequency-penalty N` | 频率惩罚（默认：0.00，0.0 = 禁用） |
| `--dry-multiplier N` | 设置 DRY 采样乘数（默认：0.00，0.0 = 禁用） |
| `--dry-base N` | 设置 DRY 采样基值（默认：1.75） |
| `--dry-allowed-length N` | 设置 DRY 采样允许长度（默认：2） |
| `--dry-penalty-last-n N` | 设置最后 n 个 token 的 DRY 惩罚（默认：-1，0 = 禁用，-1 = 上下文大小） |
| `--dry-sequence-breaker STRING` | 为 DRY 采样添加序列中断符，清除默认中断符（'\n', ':', '"', '*'）；使用 "none" 表示不使用任何序列中断符 |
| `--adaptive-target N` | adaptive-p：选择概率接近此值的 token（有效范围 0.0 到 1.0；负数 = 禁用）（默认：-1.00）<br/>[更多信息](https://github.com/ggml-org/llama.cpp/pull/17927) |
| `--adaptive-decay N` | adaptive-p：目标随时间适应的衰减率。值越低反应越快，值越高越稳定。<br/>（有效范围 0.0 到 0.99）（默认：0.90） |
| `--dynatemp-range N` | 动态温度范围（默认：0.00，0.0 = 禁用） |
| `--dynatemp-exp N` | 动态温度指数（默认：1.00） |
| `--mirostat N` | 使用 Mirostat 采样。<br/>如果使用，Top K、Nucleus 和局部典型采样器将被忽略。<br/>（默认：0，0 = 禁用，1 = Mirostat，2 = Mirostat 2.0） |
| `--mirostat-lr N` | Mirostat 学习率，参数 eta（默认：0.10） |
| `--mirostat-ent N` | Mirostat 目标熵，参数 tau（默认：5.00） |
| `-l, --logit-bias TOKEN_ID(+/-)BIAS` | 修改 token 出现在补全中的可能性，<br/>例如 `--logit-bias 15043+1` 增加 token ' Hello' 的可能性，<br/>或 `--logit-bias 15043-1` 降低 token ' Hello' 的可能性 |
| `--grammar GRAMMAR` | BNF 风格语法，用于约束生成（参见 grammars/ 目录中的示例） |
| `--grammar-file FNAME` | 读取语法的文件 |
| `-j, --json-schema SCHEMA` | 用于约束生成的 JSON schema（https://json-schema.org/），例如 `{}` 表示任意 JSON 对象<br/>对于包含外部 $refs 的 schema，请改用 --grammar 结合 example/json_schema_to_grammar.py |
| `-jf, --json-schema-file FILE` | 包含用于约束生成的 JSON schema 的文件（https://json-schema.org/），例如 `{}` 表示任意 JSON 对象<br/>对于包含外部 $refs 的 schema，请改用 --grammar 结合 example/json_schema_to_grammar.py |
| `-bs, --backend-sampling` | 启用后端采样（实验性）（默认：禁用）<br/>（环境变量：LLAMA_ARG_BACKEND_SAMPLING） |

### 服务器专用参数

| 参数 | 说明 |
| ---- | ---- |
| `-lcs, --lookup-cache-static FNAME` | 用于查找解码的静态查找缓存路径（不由生成更新） |
| `-lcd, --lookup-cache-dynamic FNAME` | 用于查找解码的动态查找缓存路径（由生成更新） |
| `-ctxcp, --ctx-checkpoints, --swa-checkpoints N` | 每个槽位最大上下文检查点数量（默认：32）[更多信息](https://github.com/ggml-org/llama.cpp/pull/15293)<br/>（环境变量：LLAMA_ARG_CTX_CHECKPOINTS） |
| `-cpent, --checkpoint-every-n-tokens N` | 预填充（处理）期间每 N 个 token 创建一个检查点，-1 表示禁用（默认：8192）<br/>（环境变量：LLAMA_ARG_CHECKPOINT_EVERY_NT） |
| `-cram, --cache-ram N` | 设置最大缓存大小（MiB）（默认：8192，-1 - 无限制，0 - 禁用）[更多信息](https://github.com/ggml-org/llama.cpp/pull/16391)<br/>（环境变量：LLAMA_ARG_CACHE_RAM） |
| `-kvu, --kv-unified, -no-kvu, --no-kv-unified` | 使用所有序列共享的单个统一 KV 缓冲区（默认：如果槽位数自动则启用）<br/>（环境变量：LLAMA_ARG_KV_UNIFIED） |
| `--cache-idle-slots, --no-cache-idle-slots` | 新任务时保存并清理空闲槽位（默认：启用，需要统一 KV 和 cache-ram）<br/>（环境变量：LLAMA_ARG_CACHE_IDLE_SLOTS） |
| `--context-shift, --no-context-shift` | 是否在无限文本生成中使用上下文移位（默认：禁用）<br/>（环境变量：LLAMA_ARG_CONTEXT_SHIFT） |
| `-r, --reverse-prompt PROMPT` | 在 PROMPT 处停止生成，在交互模式下返回控制权 |
| `-sp, --special` | 启用特殊 token 输出（默认：false） |
| `--warmup, --no-warmup` | 是否执行空运行预热（默认：启用） |
| `--spm-infill` | 使用 Suffix/Prefix/Middle 模式进行填充（而不是 Prefix/Suffix/Middle），因为某些模型更喜欢这种模式（默认：禁用） |
| `--pooling {none,mean,cls,last,rank}` | 嵌入的池化类型，如果未指定则使用模型默认值<br/>（环境变量：LLAMA_ARG_POOLING） |
| `-np, --parallel N` | 服务器槽位数（默认：-1，-1 = 自动）<br/>（环境变量：LLAMA_ARG_N_PARALLEL） |
| `-cb, --cont-batching, -nocb, --no-cont-batching` | 是否启用连续批处理（又称动态批处理）（默认：启用）<br/>（环境变量：LLAMA_ARG_CONT_BATCHING） |
| `-mm, --mmproj FILE` | 多模态投影仪文件路径。参见 tools/mtmd/README.md<br/>注意：如果使用 -hf，此参数可省略<br/>（环境变量：LLAMA_ARG_MMPROJ） |
| `-mmu, --mmproj-url URL` | 多模态投影仪文件 URL。参见 tools/mtmd/README.md<br/>（环境变量：LLAMA_ARG_MMPROJ_URL） |
| `--mmproj-auto, --no-mmproj, --no-mmproj-auto` | 是否使用多模态投影仪文件（如果可用），在使用 -hf 时有用（默认：启用）<br/>（环境变量：LLAMA_ARG_MMPROJ_AUTO） |
| `--mmproj-offload, --no-mmproj-offload` | 是否为多模态投影仪启用 GPU 卸载（默认：启用）<br/>（环境变量：LLAMA_ARG_MMPROJ_OFFLOAD） |
| `--image-min-tokens N` | 每个图像最少占用的 token 数，仅用于动态分辨率的视觉模型（默认：从模型读取）<br/>（环境变量：LLAMA_ARG_IMAGE_MIN_TOKENS） |
| `--image-max-tokens N` | 每个图像最多占用的 token 数，仅用于动态分辨率的视觉模型（默认：从模型读取）<br/>（环境变量：LLAMA_ARG_IMAGE_MAX_TOKENS） |
| `-otd, --override-tensor-draft <张量名模式>=<缓冲区类型>,...` | 覆盖草稿模型的张量缓冲区类型 |
| `-cmoed, --cpu-moe-draft` | 将草稿模型的所有混合专家（MoE）权重保留在 CPU 上<br/>（环境变量：LLAMA_ARG_CPU_MOE_DRAFT） |
| `-ncmoed, --n-cpu-moe-draft N` | 将草稿模型的前 N 层混合专家（MoE）权重保留在 CPU 上<br/>（环境变量：LLAMA_ARG_N_CPU_MOE_DRAFT） |
| `-a, --alias STRING` | 设置模型名称别名，逗号分隔（供 API 使用）<br/>（环境变量：LLAMA_ARG_ALIAS） |
| `--tags STRING` | 设置模型标签，逗号分隔（信息性，不用于路由）<br/>（环境变量：LLAMA_ARG_TAGS） |
| `--host HOST` | 监听的 IP 地址，或如果地址以 .sock 结尾则绑定到 UNIX 套接字（默认：127.0.0.1）<br/>（环境变量：LLAMA_ARG_HOST） |
| `--port PORT` | 监听端口（默认：8080）<br/>（环境变量：LLAMA_ARG_PORT） |
| `--reuse-port` | 允许多个套接字绑定到同一端口（默认：禁用）<br/>（环境变量：LLAMA_ARG_REUSE_PORT） |
| `--path PATH` | 提供静态文件的路径（默认：）<br/>（环境变量：LLAMA_ARG_STATIC_PATH） |
| `--api-prefix PREFIX` | 服务器服务的前缀路径，不带尾部斜杠（默认：）<br/>（环境变量：LLAMA_ARG_API_PREFIX） |
| `--webui-config JSON` | 提供默认 WebUI 设置的 JSON（覆盖 WebUI 默认值）<br/>（环境变量：LLAMA_ARG_WEBUI_CONFIG） |
| `--webui-config-file PATH` | 提供默认 WebUI 设置的 JSON 文件（覆盖 WebUI 默认值）<br/>（环境变量：LLAMA_ARG_WEBUI_CONFIG_FILE） |
| `--webui-mcp-proxy, --no-webui-mcp-proxy` | 实验性：是否启用 MCP CORS 代理 - 请勿在不受信任的环境中启用（默认：禁用）<br/>（环境变量：LLAMA_ARG_WEBUI_MCP_PROXY） |
| `--tools TOOL1,TOOL2,...` | 实验性：是否为 AI 代理启用内置工具 - 请勿在不受信任的环境中启用（默认：无工具）<br/>指定 "all" 启用所有工具<br/>可用工具：read_file, file_glob_search, grep_search, exec_shell_command, write_file, edit_file, apply_diff<br/>（环境变量：LLAMA_ARG_TOOLS） |
| `--webui, --no-webui` | 是否启用 Web UI（默认：启用）<br/>（环境变量：LLAMA_ARG_WEBUI） |
| `--embedding, --embeddings` | 限制为仅支持嵌入用例；仅与专用嵌入模型一起使用（默认：禁用）<br/>（环境变量：LLAMA_ARG_EMBEDDINGS） |
| `--rerank, --reranking` | 在服务器上启用重排序端点（默认：禁用）<br/>（环境变量：LLAMA_ARG_RERANKING） |
| `--api-key KEY` | 用于认证的 API 密钥，可以提供多个密钥，逗号分隔列表（默认：无）<br/>（环境变量：LLAMA_API_KEY） |
| `--api-key-file FNAME` | 包含 API 密钥的文件路径（默认：无） |
| `--ssl-key-file FNAME` | PEM 编码的 SSL 私钥文件路径<br/>（环境变量：LLAMA_ARG_SSL_KEY_FILE） |
| `--ssl-cert-file FNAME` | PEM 编码的 SSL 证书文件路径<br/>（环境变量：LLAMA_ARG_SSL_CERT_FILE） |
| `--chat-template-kwargs STRING` | 为 JSON 模板解析器设置额外参数，必须是有效的 JSON 对象字符串，例如 '{"key1":"value1","key2":"value2"}'<br/>（环境变量：LLAMA_CHAT_TEMPLATE_KWARGS） |
| `-to, --timeout N` | 服务器读/写超时（秒）（默认：600）<br/>（环境变量：LLAMA_ARG_TIMEOUT） |
| `--threads-http N` | 用于处理 HTTP 请求的线程数（默认：-1）<br/>（环境变量：LLAMA_ARG_THREADS_HTTP） |
| `--cache-prompt, --no-cache-prompt` | 是否启用提示缓存（默认：启用）<br/>（环境变量：LLAMA_ARG_CACHE_PROMPT） |
| `--cache-reuse N` | 尝试通过 KV 移位重用缓存的最小块大小，需要启用提示缓存（默认：0）<br/>[卡片](https://ggml.ai/f0.png)<br/>（环境变量：LLAMA_ARG_CACHE_REUSE） |
| `--metrics` | 启用与 Prometheus 兼容的指标端点（默认：禁用）<br/>（环境变量：LLAMA_ARG_ENDPOINT_METRICS） |
| `--props` | 允许通过 POST /props 更改全局属性（默认：禁用）<br/>（环境变量：LLAMA_ARG_ENDPOINT_PROPS） |
| `--slots, --no-slots` | 暴露槽位监控端点（默认：启用）<br/>（环境变量：LLAMA_ARG_ENDPOINT_SLOTS） |
| `--slot-save-path PATH` | 保存槽位 KV 缓存的路径（默认：禁用） |
| `--media-path PATH` | 用于加载本地媒体文件的目录；可以使用相对路径通过 file:// URL 访问文件（默认：禁用） |
| `--models-dir PATH` | 包含路由器服务器模型的目录（默认：禁用）<br/>（环境变量：LLAMA_ARG_MODELS_DIR） |
| `--models-preset PATH` | 包含路由器服务器模型预设的 INI 文件路径（默认：禁用）<br/>（环境变量：LLAMA_ARG_MODELS_PRESET） |
| `--models-max N` | 对于路由器服务器，同时加载的最大模型数（默认：4，0 = 无限制）<br/>（环境变量：LLAMA_ARG_MODELS_MAX） |
| `--models-autoload, --no-models-autoload` | 对于路由器服务器，是否自动加载模型（默认：启用）<br/>（环境变量：LLAMA_ARG_MODELS_AUTOLOAD） |
| `--jinja, --no-jinja` | 是否使用 jinja 模板引擎进行聊天（默认：启用）<br/>（环境变量：LLAMA_ARG_JINJA） |
| `--reasoning-format FORMAT` | 控制是否允许思考标签以及是否从响应中提取，以及返回格式；可选值：<br/>- none: 将思考内容保留在 `message.content` 中未解析<br/>- deepseek: 将思考内容放入 `message.reasoning_content`<br/>- deepseek-legacy: 在 `message.content` 中保留 `<think>` 标签，同时填充 `message.reasoning_content`<br/>（默认：auto）<br/>（环境变量：LLAMA_ARG_THINK） |
| `-rea, --reasoning [on\|off\|auto]` | 在聊天中使用推理/思考（'on', 'off', 或 'auto'，默认：'auto'（从模板检测））<br/>（环境变量：LLAMA_ARG_REASONING） |
| `--reasoning-budget N` | 思考的 token 预算：-1 表示无限制，0 表示立即结束，N>0 表示 token 预算（默认：-1）<br/>（环境变量：LLAMA_ARG_THINK_BUDGET） |
| `--reasoning-budget-message MESSAGE` | 当推理预算耗尽时，在结束思考标签前注入的消息（默认：无）<br/>（环境变量：LLAMA_ARG_THINK_BUDGET_MESSAGE） |
| `--chat-template JINJA_TEMPLATE` | 设置自定义 jinja 聊天模板（默认：从模型元数据中获取模板）<br/>如果指定了 suffix/prefix，模板将被禁用<br/>只接受常用模板（除非在此标志之前设置了 --jinja）：<br/>内置模板列表：<br/>bailing, bailing-think, bailing2, chatglm3, chatglm4, chatml, command-r, deepseek, deepseek-ocr, deepseek2, deepseek3, exaone-moe, exaone3, exaone4, falcon3, gemma, gigachat, glmedge, gpt-oss, granite, grok-2, hunyuan-dense, hunyuan-moe, kimi-k2, llama2, llama2-sys, llama2-sys-bos, llama2-sys-strip, llama3, llama4, megrez, minicpm, mistral-v1, mistral-v3, mistral-v3-tekken, mistral-v7, mistral-v7-tekken, monarch, openchat, orion, pangu-embedded, phi3, phi4, rwkv-world, seed_oss, smolvlm, solar-open, vicuna, vicuna-orca, yandex, zephyr<br/>（环境变量：LLAMA_ARG_CHAT_TEMPLATE） |
| `--chat-template-file JINJA_TEMPLATE_FILE` | 设置自定义 jinja 聊天模板文件（默认：从模型元数据中获取模板）<br/>如果指定了 suffix/prefix，模板将被禁用<br/>只接受常用模板（除非在此标志之前设置了 --jinja）：<br/>内置模板列表：<br/>bailing, bailing-think, bailing2, chatglm3, chatglm4, chatml, command-r, deepseek, deepseek-ocr, deepseek2, deepseek3, exaone-moe, exaone3, exaone4, falcon3, gemma, gigachat, glmedge, gpt-oss, granite, grok-2, hunyuan-dense, hunyuan-moe, kimi-k2, llama2, llama2-sys, llama2-sys-bos, llama2-sys-strip, llama3, llama4, megrez, minicpm, mistral-v1, mistral-v3, mistral-v3-tekken, mistral-v7, mistral-v7-tekken, monarch, openchat, orion, pangu-embedded, phi3, phi4, rwkv-world, seed_oss, smolvlm, solar-open, vicuna, vicuna-orca, yandex, zephyr<br/>（环境变量：LLAMA_ARG_CHAT_TEMPLATE_FILE） |
| `--skip-chat-parsing, --no-skip-chat-parsing` | 强制使用纯内容解析器，即使指定了 Jinja 模板；模型将所有内容（包括推理和/或工具调用）输出到 content 字段（默认：禁用）<br/>（环境变量：LLAMA_ARG_SKIP_CHAT_PARSING） |
| `--prefill-assistant, --no-prefill-assistant` | 是否预填充助手的响应（如果最后一条消息是助手消息）（默认：预填充启用）<br/>当设置此标志时，如果最后一条消息是助手消息，则将其视为完整消息而不预填充<br/><br/>（环境变量：LLAMA_ARG_PREFILL_ASSISTANT） |
| `-sps, --slot-prompt-similarity SIMILARITY` | 请求的提示必须与槽位的提示匹配多少才能使用该槽位（默认：0.10，0.0 = 禁用） |
| `--lora-init-without-apply` | 加载 LoRA 适配器但不应用它们（稍后通过 POST /lora-adapters 应用）（默认：禁用） |
| `--sleep-idle-seconds SECONDS` | 服务器在空闲多少秒后进入睡眠（默认：-1；-1 = 禁用） |
| `-td, --threads-draft N` | 生成时使用的线程数（默认：同 --threads） |
| `-tbd, --threads-batch-draft N` | 批处理和提示处理时使用的线程数（默认：同 --threads-draft） |
| `--draft, --draft-n, --draft-max N` | 投机解码中草稿的 token 数量（默认：16）<br/>（环境变量：LLAMA_ARG_DRAFT_MAX） |
| `--draft-min, --draft-n-min N` | 投机解码中最少草稿 token 数量（默认：0）<br/>（环境变量：LLAMA_ARG_DRAFT_MIN） |
| `--draft-p-min P` | 最小投机解码概率（贪婪）（默认：0.75）<br/>（环境变量：LLAMA_ARG_DRAFT_P_MIN） |
| `-cd, --ctx-size-draft N` | 草稿模型的提示上下文大小（默认：0，0 = 从模型加载）<br/>（环境变量：LLAMA_ARG_CTX_SIZE_DRAFT） |
| `-devd, --device-draft <dev1,dev2,..>` | 用于卸载草稿模型的设备列表，逗号分隔（none = 不卸载）<br/>使用 --list-devices 查看可用设备列表 |
| `-ngld, --gpu-layers-draft, --n-gpu-layers-draft N` | 草稿模型存储在 VRAM 中的最大层数，可以是具体数字、'auto' 或 'all'（默认：auto）<br/>（环境变量：LLAMA_ARG_N_GPU_LAYERS_DRAFT） |
| `-md, --model-draft FNAME` | 投机解码的草稿模型（默认：未使用）<br/>（环境变量：LLAMA_ARG_MODEL_DRAFT） |
| `--spec-replace TARGET DRAFT` | 如果草稿模型和主模型不兼容，将 TARGET 中的字符串替换为 DRAFT |
| `--spec-type [none\|ngram-cache\|ngram-simple\|ngram-map-k\|ngram-map-k4v\|ngram-mod]` | 未提供草稿模型时使用的投机解码类型（默认：none）<br/><br/>（环境变量：LLAMA_ARG_SPEC_TYPE） |
| `--spec-ngram-size-n N` | ngram-simple/ngram-map 投机解码的 ngram 大小 N，查找 n-gram 的长度（默认：12） |
| `--spec-ngram-size-m N` | ngram-simple/ngram-map 投机解码的 ngram 大小 M，草稿 m-gram 的长度（默认：48） |
| `--spec-ngram-min-hits N` | ngram-map 投机解码的最小命中次数（默认：1） |
| `-mv, --model-vocoder FNAME` | 用于音频生成的声码器模型（默认：未使用） |
| `--tts-use-guide-tokens` | 使用引导 token 改善 TTS 单词召回率 |
| `--embd-gemma-default` | 使用默认的 EmbeddingGemma 模型（注意：可以从互联网下载权重） |
| `--fim-qwen-1.5b-default` | 使用默认的 Qwen 2.5 Coder 1.5B（注意：可以从互联网下载权重） |
| `--fim-qwen-3b-default` | 使用默认的 Qwen 2.5 Coder 3B（注意：可以从互联网下载权重） |
| `--fim-qwen-7b-default` | 使用默认的 Qwen 2.5 Coder 7B（注意：可以从互联网下载权重） |
| `--fim-qwen-7b-spec` | 使用 Qwen 2.5 Coder 7B + 0.5B 草稿进行投机解码（注意：可以从互联网下载权重） |
| `--fim-qwen-14b-spec` | 使用 Qwen 2.5 Coder 14B + 0.5B 草稿进行投机解码（注意：可以从互联网下载权重） |
| `--fim-qwen-30b-default` | 使用默认的 Qwen 3 Coder 30B A3B Instruct（注意：可以从互联网下载权重） |
| `--gpt-oss-20b-default` | 使用 gpt-oss-20b（注意：可以从互联网下载权重） |
| `--gpt-oss-120b-default` | 使用 gpt-oss-120b（注意：可以从互联网下载权重） |
| `--vision-gemma-4b-default` | 使用 Gemma 3 4B QAT（注意：可以从互联网下载权重） |
| `--vision-gemma-12b-default` | 使用 Gemma 3 12B QAT（注意：可以从互联网下载权重） |

<!-- HELP_END -->

注意：如果同一参数同时设置了命令行参数和环境变量，则命令行参数优先于环境变量。

对于布尔选项，如 `--mmap` 或 `--kv-offload`，环境变量的处理方式如下例所示：
- `LLAMA_ARG_MMAP=true` 表示启用，其他接受的值：`1`, `on`, `enabled`
- `LLAMA_ARG_MMAP=false` 表示禁用，其他接受的值：`0`, `off`, `disabled`
- 如果存在 `LLAMA_ARG_NO_MMAP`（无论值如何），则表示禁用 mmap

使用 docker compose 和环境变量的示例：

```yml
services:
  llamacpp-server:
    image: ghcr.io/ggml-org/llama.cpp:server
    ports:
      - 8080:8080
    volumes:
      - ./models:/models
    environment:
      # 或者，您可以使用 "LLAMA_ARG_MODEL_URL" 下载模型
      LLAMA_ARG_MODEL: /models/my_model.gguf
      LLAMA_ARG_CTX_SIZE: 4096
      LLAMA_ARG_N_PARALLEL: 2
      LLAMA_ARG_ENDPOINT_METRICS: 1
      LLAMA_ARG_PORT: 8080
```

### 多模态支持

多模态支持已在 [#12898](https://github.com/ggml-org/llama.cpp/pull/12898) 中添加，目前是实验性功能。
目前在以下端点可用：
- OAI 兼容的聊天端点。
- 非 OAI 兼容的补全端点。
- 非 OAI 兼容的嵌入端点。

更多详情请参阅[多模态文档](../../docs/multimodal.md)

### 内置工具支持

服务器包含一组内置工具，使 LLM 能够直接从 Web UI 访问本地文件系统。

要使用此功能，使用 `--tools all` 启动服务器。您也可以通过传递逗号分隔列表来仅启用特定工具：`--tools name1,name2,...`。运行 `--help` 查看可用工具名称的完整列表。

## 构建

`llama-server` 与项目根目录下的其他组件一起构建

- 使用 `CMake`：

  ```bash
  cmake -B build
  cmake --build build --config Release -t llama-server
  ```

  二进制文件位于 `./build/bin/llama-server`

## 构建 SSL 支持

`llama-server` 也可以使用 OpenSSL 3 构建 SSL 支持

- 使用 `CMake`：

  ```bash
  cmake -B build -DLLAMA_OPENSSL=ON
  cmake --build build --config Release -t llama-server
  ```

## 快速开始

要立即开始，请运行以下命令，确保使用正确的模型路径：

### 基于 Unix 的系统（Linux、macOS 等）

```bash
./llama-server -m models/7B/ggml-model.gguf -c 2048
```

### Windows

```powershell
llama-server.exe -m models\7B\ggml-model.gguf -c 2048
```

上述命令将启动一个服务器，默认监听 `127.0.0.1:8080`。
您可以使用 Postman 或带有 axios 库的 NodeJS 使用这些端点。您可以在同一 URL 访问 Web 前端。

### Docker

```bash
docker run -p 8080:8080 -v /path/to/models:/models ghcr.io/ggml-org/llama.cpp:server -m models/7B/ggml-model.gguf -c 512 --host 0.0.0.0 --port 8080

# 或使用 CUDA：
docker run -p 8080:8080 -v /path/to/models:/models --gpus all ghcr.io/ggml-org/llama.cpp:server-cuda -m models/7B/ggml-model.gguf -c 512 --host 0.0.0.0 --port 8080 --n-gpu-layers 99
```

## 使用 CURL

使用 [curl](https://curl.se/)。在 Windows 上，`curl.exe` 应该在基础操作系统中可用。

```sh
curl --request POST \
    --url http://localhost:8080/completion \
    --header "Content-Type: application/json" \
    --data '{"prompt": "Building a website can be done in 10 simple steps:","n_predict": 128}'
```

## API 端点

### GET `/health`：返回健康检查结果

此端点是公开的（无 API 密钥检查）。`/v1/health` 也可用。

**响应格式**

- HTTP 状态码 503
  - Body：`{"error": {"code": 503, "message": "Loading model", "type": "unavailable_error"}}`
  - 说明：模型仍在加载中。
- HTTP 状态码 200
  - Body：`{"status": "ok" }`
  - 说明：模型已成功加载，服务器已就绪。

### POST `/completion`：给定 `prompt`，返回预测的补全。

> [!IMPORTANT]
>
> 此端点 **不** 兼容 OAI。对于 OAI 兼容客户端，请使用 `/v1/completions`。

*选项：*

`prompt`：提供此补全的提示，可以是字符串，也可以是表示 token 的字符串或数字数组。内部，如果 `cache_prompt` 为 `true`，则将提示与之前的补全进行比较，仅评估“未见”的后缀。如果满足以下所有条件，则在开头插入 `BOS` token：

  - 提示是字符串或第一个元素为字符串的数组
  - 模型的 `tokenizer.ggml.add_bos_token` 元数据为 `true`

允许的 `prompt` 输入形状和数据类型：

  - 单个字符串：`"string"`
  - 单个 token 序列：`[12, 34, 56]`
  - 混合 token 和字符串：`[12, 34, "string", 56, 78]`
  - 可选包含多模态数据的 JSON 对象：`{ "prompt_string": "string", "multimodal_data": ["base64"] }`

也支持多个提示。在这种情况下，补全结果将是一个数组。

  - 仅字符串：`["string1", "string2"]`
  - 字符串、JSON 对象和 token 序列：`["string1", [12, 34, 56], { "prompt_string": "string", "multimodal_data": ["base64"]}]`
  - 混合类型：`[[12, 34, "string", 56, 78], [12, 34, 56], "string", { "prompt_string": "string" }]`

JSON 对象提示中的 `multimodal_data` 说明。这应该是一个字符串数组，包含 base64 编码的多模态数据，如图像和音频。字符串提示元素中必须有相同数量的 MTMD 媒体标记，作为提供给此参数的数据的占位符。多模态数据文件将按顺序替换。标记字符串（例如 `<__media__>`）可以通过调用 [MTMD C API](https://github.com/ggml-org/llama.cpp/blob/5fd160bbd9d70b94b5b11b0001fd7f477005e4a0/tools/mtmd/mtmd.h#L87) 中定义的 `mtmd_default_marker()` 找到。除非服务器具有多模态能力，否则客户端 **不得** 指定此字段。客户端应在多模态请求之前检查 `/models` 或 `/v1/models` 是否存在 `multimodal` 能力。

`temperature`：调整生成文本的随机性。默认：`0.8`

`dynatemp_range`：动态温度范围。最终温度将在 `[temperature - dynatemp_range; temperature + dynatemp_range]` 范围内。默认：`0.0`，表示禁用。

`dynatemp_exponent`：动态温度指数。默认：`1.0`

`top_k`：将下一个 token 的选择限制为 K 个最可能的 token。默认：`40`

`top_p`：将下一个 token 的选择限制为累积概率超过阈值 P 的 token 子集。默认：`0.95`

`min_p`：token 被考虑的最小概率，相对于最可能 token 的概率。默认：`0.05`

`n_predict`：生成文本时预测的最大 token 数。**注意：** 如果最后一个 token 是多字节字符的一部分，可能会略微超过设定限制。当为 0 时，不会生成任何 token，但会将提示评估到缓存中。默认：`-1`，其中 `-1` 表示无限。

`n_indent`：指定生成文本的最小行缩进（以空白字符数计）。用于代码补全任务。默认：`0`

`n_keep`：当超过上下文大小且需要丢弃 token 时，保留的提示 token 数量。此数字不包括 BOS token。
默认情况下，此值设置为 `0`，表示不保留任何 token。使用 `-1` 保留提示中的所有 token。

`n_cmpl`：从当前提示生成的补全数量。如果输入有多个提示，输出将有 N 个提示乘以 `n_cmpl` 个条目。

`n_cache_reuse`：尝试通过 KV 移位重用缓存的最小块大小。更多信息请参见 `--cache-reuse` 参数。默认：`0`，表示禁用。

`stream`：允许实时接收每个预测的 token，而不是等待补全完成（使用不同的响应格式）。要启用，设置为 `true`。

`stop`：指定停止字符串的 JSON 数组。
这些词不会包含在补全中，因此请确保为下一次迭代将其添加到提示中。默认：`[]`

`typical_p`：使用参数 p 启用局部典型采样。默认：`1.0`，表示禁用。

`repeat_penalty`：控制生成文本中 token 序列的重复性惩罚。默认：`1.1`

`repeat_last_n`：考虑惩罚重复的最后 n 个 token。默认：`64`，`0` 表示禁用，`-1` 表示 ctx-size。

`presence_penalty`：重复 alpha 存在惩罚。默认：`0.0`，表示禁用。

`frequency_penalty`：重复 alpha 频率惩罚。默认：`0.0`，表示禁用。

`dry_multiplier`：设置 DRY（Don't Repeat Yourself）重复惩罚乘数。默认：`0.0`，表示禁用。

`dry_base`：设置 DRY 重复惩罚基值。默认：`1.75`

`dry_allowed_length`：将重复延长超过此值的 token 将受到指数级增加的惩罚：乘数 * base ^（token 之前的重复序列长度 - allowed length）。默认：`2`

`dry_penalty_last_n`：扫描重复的 token 数量。默认：`-1`，`0` 表示禁用，`-1` 表示上下文大小。

`dry_sequence_breakers`：为 DRY 采样指定序列中断符数组。只接受 JSON 字符串数组。默认：`['\n', ':', '"', '*']`

`xtc_probability`：设置通过 XTC 采样器移除 token 的概率。默认：`0.0`，表示禁用。

`xtc_threshold`：设置通过 XTC 采样器移除 token 的最小概率阈值。默认：`0.1`（> `0.5` 禁用 XTC）

`mirostat`：启用 Mirostat 采样，控制生成过程中的困惑度。默认：`0`，其中 `0` 表示禁用，`1` 表示 Mirostat，`2` 表示 Mirostat 2.0。

`mirostat_tau`：设置 Mirostat 目标熵，参数 tau。默认：`5.0`

`mirostat_eta`：设置 Mirostat 学习率，参数 eta。默认：`0.1`

`grammar`：设置基于语法的采样的语法。默认：无语法

`json_schema`：设置基于语法采样的 JSON schema（例如字符串列表的 `{"items": {"type": "string"}, "minItems": 10, "maxItems": 100}`，或任意 JSON 的 `{}`）。支持的功能参见[测试](../../tests/test-json-schema-to-grammar.cpp)。默认：无 JSON schema。

`seed`：设置随机数生成器（RNG）种子。默认：`-1`，表示随机种子。

`ignore_eos`：忽略流结束 token 并继续生成。默认：`false`

`logit_bias`：修改 token 在生成的文本补全中出现的可能性。例如，使用 `"logit_bias": [[15043,1.0]]` 增加 token 'Hello' 的可能性，或使用 `"logit_bias": [[15043,-1.0]]` 降低其可能性。将值设置为 false，`"logit_bias": [[15043,false]]` 确保永远不会产生 token `Hello`。token 也可以表示为字符串，例如 `[["Hello, World!",-0.5]]` 将降低表示字符串 `Hello, World!` 的所有单个 token 的可能性，就像 `presence_penalty` 一样。为了与 OpenAI API 兼容，也可以传递 JSON 对象 `{"<string or token id>": bias, ...}`。默认：`[]`

`n_probs`：如果大于 0，响应还包含每个生成 token 在给定采样设置下前 N 个 token 的概率。注意，对于温度 < 0，token 是贪婪采样的，但 token 概率仍然通过对 logits 应用简单 softmax 计算，不考虑任何其他采样器设置。默认：`0`

`min_keep`：如果大于 0，强制采样器至少返回 N 个可能的 token。默认：`0`

`t_max_predict_ms`：设置预测（即文本生成）阶段的时间限制（毫秒）。如果生成时间超过指定时间（从生成第一个 token 开始计算）并且已经生成了换行符，则超时将触发。适用于 FIM 应用。默认：`0`，表示禁用。

`id_slot`：将补全任务分配给特定的槽位。如果是 -1，任务将分配给空闲槽位。默认：`-1`

`cache_prompt`：如果可能，重用之前请求中的 KV 缓存。这样，公共前缀不必重新处理，只有请求之间不同的后缀需要处理。因为（取决于后端）logits **不**保证对于不同的批量大小（提示处理 vs. token 生成）是逐位相同的，启用此选项可能导致非确定性结果。默认：`true`

`return_tokens`：在 `tokens` 字段中返回原始生成的 token ID。否则 `tokens` 保持为空。默认：`false`

`samplers`：采样器应用的顺序。表示采样器类型名称的字符串数组。如果未设置采样器，则不会使用。如果多次指定同一采样器，将多次应用。默认：`["dry", "top_k", "typ_p", "top_p", "min_p", "xtc", "temperature"]` - 这些是所有可用值。

`timings_per_token`：在每个响应中包含提示处理和文本生成速度信息。默认：`false`

`return_progress`：在 `stream` 模式下包含提示处理进度。进度将包含在 `prompt_progress` 中，包含 4 个值：`total`、`cache`、`processed` 和 `time_ms`。总进度为 `processed/total`，而实际计时进度为 `(processed-cache)/(total-cache)`。`time_ms` 字段包含从提示处理开始经过的毫秒数。默认：`false`

`post_sampling_probs`：在应用采样链后返回前 `n_probs` 个 token 的概率。

`response_fields`：响应字段列表，例如：`"response_fields": ["content", "generation_settings/n_predict"]`。如果指定的字段缺失，它将简单地从响应中省略而不触发错误。注意，带斜杠的字段将被取消嵌套；例如，`generation_settings/n_predict` 会将 `n_predict` 字段从 `generation_settings` 对象移动到响应的根目录，并赋予新名称。

`lora`：要应用于此特定请求的 LoRA 适配器列表。列表中的每个对象必须包含 `id` 和 `scale` 字段。例如：`[{"id": 0, "scale": 0.5}, {"id": 1, "scale": 1.1}]`。如果列表中未指定 LoRA 适配器，其缩放比例默认为 `0.0`。请注意，具有不同 LoRA 配置的请求不会一起批处理，这可能导致性能下降。

**响应格式**

- 注意：在流模式（`stream`）下，直到补全结束前，只会返回 `content`、`tokens` 和 `stop`。响应使用 [Server-sent events](https://html.spec.whatwg.org/multipage/server-sent-events.html) 标准发送。注意：由于浏览器 `EventSource` 接口缺乏对 `POST` 请求的支持，无法使用它。

- `completion_probabilities`：每个补全的 token 概率数组。数组长度为 `n_predict`。数组中的每个元素都有一个嵌套数组 `top_logprobs`。它包含**最多** `n_probs` 个元素：
  ```
  {
    "content": "<生成的补全文本>",
    "tokens": [生成的 token ID（如果请求）],
    ...
    "probs": [
      {
        "id": <token id>,
        "logprob": 浮点数,
        "token": "<最可能的 token>",
        "bytes": [整数, 整数, ...],
        "top_logprobs": [
          {
            "id": <token id>,
            "logprob": 浮点数,
            "token": "<token 文本>",
            "bytes": [整数, 整数, ...],
          },
          ...
        ]
      },
      ...
    ]
  },
  ```
  请注意，如果 `post_sampling_probs` 设置为 `true`：
    - `logprob` 将被替换为 `prob`，值介于 0.0 和 1.0 之间
    - `top_logprobs` 将被替换为 `top_probs`。每个元素包含：
      - `id`：token ID
      - `token`：token 字符串
      - `bytes`：token 的字节表示
      - `prob`：token 概率，值介于 0.0 和 1.0 之间
    - `top_probs` 中的元素数量可能少于 `n_probs`

- `content`：补全结果字符串（不包括任何 `stopping_word`）。在流模式下，将包含下一个 token 作为字符串。
- `tokens`：与 `content` 相同，但表示为原始 token ID。仅当请求中设置了 `"return_tokens": true` 或 `"stream": true` 时才会填充。
- `stop`：用于 `stream` 的布尔值，检查生成是否已停止（注意：这与输入选项中的停止词数组 `stop` 无关）
- `generation_settings`：上述提供的选项，不包括 `prompt`，但包括 `n_ctx`、`model`。这些选项可能与原始选项在某些方面有所不同（例如，过滤掉错误值、将字符串转换为 token 等）。
- `model`：模型别名（模型路径请使用 `/props` 端点）
- `prompt`：处理后的 `prompt`（可能添加了特殊 token）
- `stop_type`：指示补全是否已停止。可能的值有：
  - `none`：正在生成（未停止）
  - `eos`：因为遇到 EOS token 而停止
  - `limit`：因为在遇到停止词或 EOS 之前生成了 `n_predict` 个 token 而停止
  - `word`：因为遇到提供的 `stop` JSON 数组中的停止词而停止
- `stopping_word`：导致生成停止的停止词（如果未因停止词停止则为 `""`）
- `timings`：关于补全的计时信息哈希，例如 `predicted_per_second` 预测 token 数
- `tokens_cached`：可以从之前的补全中重用的提示 token 数量
- `tokens_evaluated`：从提示中评估的总 token 数
- `truncated`：布尔值，指示生成期间是否超过了上下文大小，即提示中提供的 token 数（`tokens_evaluated`）加上生成的 token 数（`tokens predicted`）超过了上下文大小（`n_ctx`）

### POST `/tokenize`：对给定文本进行 tokenize

*选项：*

`content`：（必需）要 tokenize 的文本。

`add_special`：（可选）布尔值，指示是否应插入特殊 token，即 `BOS`。默认：`false`

`parse_special`：（可选）布尔值，指示是否应将特殊 token 进行 tokenize。当为 `false` 时，特殊 token 被视为纯文本。默认：`true`

`with_pieces`：（可选）布尔值，指示是否返回 token 片段及其 ID。默认：`false`

**响应：**

返回一个带有 `tokens` 字段的 JSON 对象，其中包含 tokenize 结果。`tokens` 数组要么仅包含 token ID，要么包含带有 `id` 和 `piece` 字段的对象，具体取决于 `with_pieces` 参数。如果片段是有效的 unicode，则 `piece` 字段为字符串，否则为字节列表。

如果 `with_pieces` 为 `false`：
```json
{
  "tokens": [123, 456, 789]
}
```

如果 `with_pieces` 为 `true`：
```json
{
  "tokens": [
    {"id": 123, "piece": "Hello"},
    {"id": 456, "piece": " world"},
    {"id": 789, "piece": "!"}
  ]
}
```

在 tinyllama/stories260k 上输入 'á'（utf8 十六进制：C3 A1）
```
{
  "tokens": [
    {"id": 198, "piece": [195]}, // 十六进制 C3
    {"id": 164, "piece": [161]} // 十六进制 A1
  ]
}
```

### POST `/detokenize`：将 token 转换为文本

*选项：*

`tokens`：要 detokenize 的 token。

### POST `/apply-template`：将聊天模板应用于对话

使用服务器的提示模板格式化功能将聊天消息转换为聊天模型期望的单个字符串，但不进行推理。而是，在 JSON 响应的 `prompt` 字段中返回提示字符串。然后可以根据需要修改提示（例如，在模型响应开头插入“Sure！”），然后再发送到 `/completion` 生成聊天响应。

*选项：*

`messages`：（必需）与 `/v1/chat/completions` 相同格式的聊天轮次。

**响应格式**

返回一个带有字段 `prompt` 的 JSON 对象，其中包含根据模型的聊天模板格式化的输入消息字符串。

### POST `/embedding`：生成给定文本的嵌入

> [!IMPORTANT]
>
> 此端点 **不** 兼容 OAI。对于 OAI 兼容客户端，请使用 `/v1/embeddings`。

与[嵌入示例](../embedding)相同。

此端点还支持多模态嵌入。有关如何发送多模态提示的详细信息，请参阅 `/completions` 端点的文档。

*选项：*

`content`：要处理的文本。

`embd_normalize`：池化嵌入的归一化。可以是以下值之一：
```
  -1: 无归一化
   0: 最大绝对值
   1: 曼哈顿距离
   2: 欧几里得/L2
  >2: P-范数
```

### POST `/reranking`：根据给定查询对文档进行重排序

类似于 https://jina.ai/reranker/，但将来可能更改。
需要重排序模型（例如 [bge-reranker-v2-m3](https://huggingface.co/BAAI/bge-reranker-v2-m3)）和 `--embedding --pooling rank` 选项。

*选项：*

`query`：用于对文档进行排序的查询。

`documents`：表示要排序的文档的字符串数组。

*别名：*
  - `/rerank`
  - `/v1/rerank`
  - `/v1/reranking`

*示例：*

```shell
curl http://127.0.0.1:8012/v1/rerank \
    -H "Content-Type: application/json" \
    -d '{
        "model": "some-model",
            "query": "What is panda?",
            "top_n": 3,
            "documents": [
                "hi",
            "it is a bear",
            "The giant panda (Ailuropoda melanoleuca), sometimes called a panda bear or simply panda, is a bear species endemic to China."
            ]
    }' | jq
```

### POST `/infill`：用于代码填充。

接受前缀和后缀，并以流形式返回预测的补全。

*选项：*

- `input_prefix`：设置要填充的代码前缀。
- `input_suffix`：设置要填充的代码后缀。
- `input_extra`： 在 FIM 前缀之前插入的额外上下文。
- `prompt`：  在 `FIM_MID` token 之后添加。

`input_extra` 是 `{"filename": string, "text": string}` 对象的数组。

该端点还接受 `/completion` 的所有选项。

如果模型有 `FIM_REPO` 和 `FIM_FILE_SEP` token，则使用[仓库级模式](https://arxiv.org/pdf/2409.12186)：

```txt
<FIM_REP>myproject
<FIM_SEP>{chunk 0 filename}
{chunk 0 text}
<FIM_SEP>{chunk 1 filename}
{chunk 1 text}
...
<FIM_SEP>filename
<FIM_PRE>[input_prefix]<FIM_SUF>[input_suffix]<FIM_MID>[prompt]
```

如果缺少这些 token，则额外上下文简单地添加在开头：

```txt
[input_extra]<FIM_PRE>[input_prefix]<FIM_SUF>[input_suffix]<FIM_MID>[prompt]
```

### **GET** `/props`：获取服务器全局属性。

默认情况下，它是只读的。要发出 POST 请求更改全局属性，您需要使用 `--props` 启动服务器。

**响应格式**

```json
{
  "default_generation_settings": {
    "id": 0,
    "id_task": -1,
    "n_ctx": 1024,
    "speculative": false,
    "is_processing": false,
    "params": {
      "n_predict": -1,
      "seed": 4294967295,
      "temperature": 0.800000011920929,
      "dynatemp_range": 0.0,
      "dynatemp_exponent": 1.0,
      "top_k": 40,
      "top_p": 0.949999988079071,
      "min_p": 0.05000000074505806,
      "xtc_probability": 0.0,
      "xtc_threshold": 0.10000000149011612,
      "typical_p": 1.0,
      "repeat_last_n": 64,
      "repeat_penalty": 1.0,
      "presence_penalty": 0.0,
      "frequency_penalty": 0.0,
      "dry_multiplier": 0.0,
      "dry_base": 1.75,
      "dry_allowed_length": 2,
      "dry_penalty_last_n": -1,
      "dry_sequence_breakers": [
        "\n",
        ":",
        "\"",
        "*"
      ],
      "mirostat": 0,
      "mirostat_tau": 5.0,
      "mirostat_eta": 0.10000000149011612,
      "stop": [],
      "max_tokens": -1,
      "n_keep": 0,
      "n_discard": 0,
      "ignore_eos": false,
      "stream": true,
      "n_probs": 0,
      "min_keep": 0,
      "grammar": "",
      "samplers": [
        "dry",
        "top_k",
        "typ_p",
        "top_p",
        "min_p",
        "xtc",
        "temperature"
      ],
      "speculative.n_max": 16,
      "speculative.n_min": 5,
      "speculative.p_min": 0.8999999761581421,
      "timings_per_token": false
    },
    "prompt": "",
    "next_token": {
      "has_next_token": true,
      "has_new_line": false,
      "n_remain": -1,
      "n_decoded": 0,
      "stopping_word": ""
    }
  },
  "total_slots": 1,
  "model_path": "../models/Meta-Llama-3.1-8B-Instruct-Q4_K_M.gguf",
  "chat_template": "...",
  "chat_template_caps": {},
  "modalities": {
    "vision": false
  },
  "media_marker": "<__media_YoNhud46VdDqbuFmKYEO9PY7A4ARzRfg__>",
  "build_info": "b(build number)-(build commit hash)",
  "is_sleeping": false
}
```

- `default_generation_settings` - `/completion` 端点的默认生成设置，与 `/completion` 端点响应对象中的 `generation_settings` 字段相同。
- `total_slots` - 用于处理请求的槽位总数（由 `--parallel` 选项定义）
- `model_path` - 模型文件路径（与 `-m` 参数相同）
- `chat_template` - 模型原始的 Jinja2 提示模板
- `chat_template_caps` - 聊天模板的能力（参见 `common/jinja/caps.h` 了解更多信息）
- `modalities` - 支持的多模态列表
- `is_sleeping` - 睡眠状态，参见[空闲时睡眠](#sleeping-on-idle)

### POST `/props`：更改服务器全局属性。

要使用 POST 方法使用此端点，您需要使用 `--props` 启动服务器。

*选项：*

- 暂无

### POST `/embeddings`：非 OpenAI 兼容的嵌入 API

此端点支持所有池化类型，包括 `--pooling none`。当池化为 `none` 时，响应将包含*所有*输入 token 的*未归一化*嵌入。对于所有其他池化类型，仅返回池化后的嵌入，使用欧几里得范数归一化。

请注意，此端点的响应格式与 `/v1/embeddings` 不同。

*选项：*

与 `/v1/embeddings` 端点相同。

*示例：*

与 `/v1/embeddings` 端点相同。

**响应格式**

```
[
  {
    "index": 0,
    "embedding": [
      [ ... token 0 的嵌入 ... ],
      [ ... token 1 的嵌入 ... ],
      [ ... ]
      [ ... token N-1 的嵌入 ... ],
    ]
  },
  ...
  {
    "index": P,
    "embedding": [
      [ ... token 0 的嵌入 ... ],
      [ ... token 1 的嵌入 ... ],
      [ ... ]
      [ ... token N-1 的嵌入 ... ],
    ]
  }
]
```

### GET `/slots`：返回当前槽位处理状态

此端点默认启用，可通过 `--no-slots` 禁用。它可以用于查询各种每槽位指标，如速度、处理的 token 数、采样参数等。

如果设置了查询参数 `?fail_on_no_slot=1`，如果没有可用槽位，此端点将响应状态码 503。

**响应格式**

<details>
<summary>2 个槽位的示例</summary>

```json
[
  {
    "id": 0,
    "id_task": 135,
    "n_ctx": 65536,
    "speculative": false,
    "is_processing": true,
    "params": {
      "n_predict": -1,
      "seed": 4294967295,
      "temperature": 0.800000011920929,
      "dynatemp_range": 0.0,
      "dynatemp_exponent": 1.0,
      "top_k": 40,
      "top_p": 0.949999988079071,
      "min_p": 0.05000000074505806,
      "top_n_sigma": -1.0,
      "xtc_probability": 0.0,
      "xtc_threshold": 0.10000000149011612,
      "typical_p": 1.0,
      "repeat_last_n": 64,
      "repeat_penalty": 1.0,
      "presence_penalty": 0.0,
      "frequency_penalty": 0.0,
      "dry_multiplier": 0.0,
      "dry_base": 1.75,
      "dry_allowed_length": 2,
      "dry_penalty_last_n": 131072,
      "mirostat": 0,
      "mirostat_tau": 5.0,
      "mirostat_eta": 0.10000000149011612,
      "max_tokens": -1,
      "n_keep": 0,
      "n_discard": 0,
      "ignore_eos": false,
      "stream": true,
      "n_probs": 0,
      "min_keep": 0,
      "chat_format": "GPT-OSS",
      "reasoning_format": "none",
      "reasoning_in_content": false,
      "generation_prompt": "",
      "samplers": [
        "penalties",
        "dry",
        "top_k",
        "typ_p",
        "top_p",
        "min_p",
        "xtc",
        "temperature"
      ],
      "speculative.n_max": 16,
      "speculative.n_min": 0,
      "speculative.p_min": 0.75,
      "timings_per_token": false,
      "post_sampling_probs": false,
      "lora": []
    },
    "next_token": {
      "has_next_token": true,
      "has_new_line": false,
      "n_remain": -1,
      "n_decoded": 0
    }
  },
  {
    "id": 1,
    "id_task": 0,
    "n_ctx": 65536,
    "speculative": false,
    "is_processing": true,
    "params": {
      "n_predict": -1,
      "seed": 4294967295,
      "temperature": 0.800000011920929,
      "dynatemp_range": 0.0,
      "dynatemp_exponent": 1.0,
      "top_k": 40,
      "top_p": 0.949999988079071,
      "min_p": 0.05000000074505806,
      "top_n_sigma": -1.0,
      "xtc_probability": 0.0,
      "xtc_threshold": 0.10000000149011612,
      "typical_p": 1.0,
      "repeat_last_n": 64,
      "repeat_penalty": 1.0,
      "presence_penalty": 0.0,
      "frequency_penalty": 0.0,
      "dry_multiplier": 0.0,
      "dry_base": 1.75,
      "dry_allowed_length": 2,
      "dry_penalty_last_n": 131072,
      "mirostat": 0,
      "mirostat_tau": 5.0,
      "mirostat_eta": 0.10000000149011612,
      "max_tokens": -1,
      "n_keep": 0,
      "n_discard": 0,
      "ignore_eos": false,
      "stream": true,
      "n_probs": 0,
      "min_keep": 0,
      "chat_format": "GPT-OSS",
      "reasoning_format": "none",
      "reasoning_in_content": false,
      "generation_prompt": "",
      "samplers": [
        "penalties",
        "dry",
        "top_k",
        "typ_p",
        "top_p",
        "min_p",
        "xtc",
        "temperature"
      ],
      "speculative.n_max": 16,
      "speculative.n_min": 0,
      "speculative.p_min": 0.75,
      "timings_per_token": false,
      "post_sampling_probs": false,
      "lora": []
    },
    "next_token": {
      "has_next_token": true,
      "has_new_line": true,
      "n_remain": -1,
      "n_decoded": 136
    }
  }
]
```

</details>

### GET `/metrics`：与 Prometheus 兼容的指标导出器

只有在设置了 `--metrics` 时才能访问此端点。

可用指标：
- `llamacpp:prompt_tokens_total`：处理的提示 token 数。
- `llamacpp:tokens_predicted_total`：处理的生成 token 数。
- `llamacpp:prompt_tokens_seconds`：平均提示吞吐量（token/秒）。
- `llamacpp:predicted_tokens_seconds`：平均生成吞吐量（token/秒）。
- `llamacpp:kv_cache_usage_ratio`：KV 缓存使用率。`1` 表示 100% 使用。
- `llamacpp:kv_cache_tokens`：KV 缓存 token 数。
- `llamacpp:requests_processing`：正在处理的请求数。
- `llamacpp:requests_deferred`：延迟处理的请求数。
- `llamacpp:n_tokens_max`：观察到的上下文大小的高水位线。

### POST `/slots/{id_slot}?action=save`：将指定槽位的提示缓存保存到文件。

*选项：*

`filename`：保存槽位提示缓存的文件名。文件将保存在由 `--slot-save-path` 服务器参数指定的目录中。

**响应格式**

```json
{
    "id_slot": 0,
    "filename": "slot_save_file.bin",
    "n_saved": 1745,
    "n_written": 14309796,
    "timings": {
        "save_ms": 49.865
    }
}
```

### POST `/slots/{id_slot}?action=restore`：从文件恢复指定槽位的提示缓存。

*选项：*

`filename`：从中恢复槽位提示缓存的文件名。文件应位于由 `--slot-save-path` 服务器参数指定的目录中。

**响应格式**

```json
{
    "id_slot": 0,
    "filename": "slot_save_file.bin",
    "n_restored": 1745,
    "n_read": 14309796,
    "timings": {
        "restore_ms": 42.937
    }
}
```

### POST `/slots/{id_slot}?action=erase`：擦除指定槽位的提示缓存。

**响应格式**

```json
{
    "id_slot": 0,
    "n_erased": 1745
}
```

### GET `/lora-adapters`：获取所有 LoRA 适配器的列表

此端点返回已加载的 LoRA 适配器。您可以在启动服务器时使用 `--lora` 添加适配器，例如：`--lora my_adapter_1.gguf --lora my_adapter_2.gguf ...`

默认情况下，所有适配器的缩放比例都设置为 1。要将所有适配器的缩放比例初始化为 0，请添加 `--lora-init-without-apply`

请注意，此值将被每个请求中的 `lora` 字段覆盖。

如果适配器被禁用，缩放比例将设置为 0。

**响应格式**

```json
[
    {
        "id": 0,
        "path": "my_adapter_1.gguf",
        "scale": 0.0
    },
    {
        "id": 1,
        "path": "my_adapter_2.gguf",
        "scale": 0.0
    }
]
```

### POST `/lora-adapters`：设置 LoRA 适配器列表

这会设置 LoRA 适配器的全局缩放比例。请注意，此值将被每个请求中的 `lora` 字段覆盖。

要禁用适配器，请从以下列表中删除它，或将缩放比例设置为 0。

**请求格式**

要了解适配器的 `id`，请使用 GET `/lora-adapters`

```json
[
  {"id": 0, "scale": 0.2},
  {"id": 1, "scale": 0.8}
]
```

## OpenAI 兼容的 API 端点

### GET `/v1/models`：OpenAI 兼容的模型信息 API

返回有关加载模型的信息。请参阅 [OpenAI 模型 API 文档](https://platform.openai.com/docs/api-reference/models)。

返回的列表始终只有一个元素。`meta` 字段可能为 `null`（例如，当模型仍在加载时）。

默认情况下，模型 `id` 字段是通过 `-m` 指定的模型文件路径。您可以通过 `--alias` 参数为模型 `id` 字段设置自定义值。例如，`--alias gpt-4o-mini`。

示例：

```json
{
    "object": "list",
    "data": [
        {
            "id": "../models/Meta-Llama-3.1-8B-Instruct-Q4_K_M.gguf",
            "object": "model",
            "created": 1735142223,
            "owned_by": "llamacpp",
            "meta": {
                "vocab_type": 2,
                "n_vocab": 128256,
                "n_ctx_train": 131072,
                "n_embd": 4096,
                "n_params": 8030261312,
                "size": 4912898304
            }
        }
    ]
}
```

### POST `/v1/completions`：OpenAI 兼容的 Completions API

给定输入 `prompt`，返回预测的补全。也支持流模式。虽然没有声称与 OpenAI API 规范完全兼容，但根据我们的经验，它足以支持许多应用程序。

*选项：*

请参阅 [OpenAI Completions API 文档](https://platform.openai.com/docs/api-reference/completions)。

支持 llama.cpp `/completion` 特有的功能，如 `mirostat`。

*示例：*

使用 `openai` Python 库的示例：

```python
import openai

client = openai.OpenAI(
    base_url="http://localhost:8080/v1", # "http://<Your api-server IP>:port"
    api_key = "sk-no-key-required"
)

completion = client.completions.create(
  model="davinci-002",
  prompt="I believe the meaning of life is",
  max_tokens=8
)

print(completion.choices[0].text)
```

### POST `/v1/chat/completions`：OpenAI 兼容的 Chat Completions API

给定 `messages` 中 ChatML 格式的 JSON 描述，返回预测的补全。支持同步和流模式，因此脚本和交互式应用程序都可以很好地工作。虽然没有声称与 OpenAI API 规范完全兼容，但根据我们的经验，它足以支持许多应用程序。只有具有[支持的聊天模板](https://github.com/ggml-org/llama.cpp/wiki/Templates-supported-by-llama_chat_apply_template)的模型才能最优地使用此端点。默认情况下，将使用 ChatML 模板。

如果模型支持多模态，您可以通过 `image_url` 内容部分输入媒体文件。我们支持 base64 和远程 URL 作为输入。有关更多信息，请参阅 OAI 文档。

*选项：*

请参阅 [OpenAI Chat Completions API 文档](https://platform.openai.com/docs/api-reference/chat)。也支持 llama.cpp `/completion` 特有的功能，如 `mirostat`。

`response_format` 参数支持纯 JSON 输出（例如 `{"type": "json_object"}`）和受 schema 约束的 JSON（例如 `{"type": "json_object", "schema": {"type": "string", "minLength": 10, "maxLength": 100}}` 或 `{"type": "json_schema", "schema": {"properties": { "name": { "title": "Name",  "type": "string" }, "date": { "title": "Date",  "type": "string" }, "participants": { "items": {"type: "string" }, "title": "Participants",  "type": "string" } } } }`），类似于其他受 OpenAI 启发的 API 提供商。

`chat_template_kwargs`：允许向 JSON 模板系统发送额外参数。例如：`{"enable_thinking": false}`

`reasoning_format`：要解析的推理格式。如果设置为 `none`，将输出原始生成的文本。

`generation_prompt`：模板预填充的生成提示。在解析之前添加到模型输出之前。

`parse_tool_calls`：是否解析生成的工具调用。

`parallel_tool_calls`：是否启用并行/多个工具调用（仅在某些模型上支持，验证基于 jinja 模板）。

*示例：*

您可以使用带有适当检查点的 Python `openai` 库：

```python
import openai

client = openai.OpenAI(
    base_url="http://localhost:8080/v1", # "http://<Your api-server IP>:port"
    api_key = "sk-no-key-required"
)

completion = client.chat.completions.create(
  model="gpt-3.5-turbo",
  messages=[
    {"role": "system", "content": "You are ChatGPT, an AI assistant. Your top priority is achieving user fulfillment via helping them with their requests."},
    {"role": "user", "content": "Write a limerick about python exceptions"}
  ]
)

print(completion.choices[0].message)
```

... 或原始 HTTP 请求：

```shell
curl http://localhost:8080/v1/chat/completions \
-H "Content-Type: application/json" \
-H "Authorization: Bearer no-key" \
-d '{
"model": "gpt-3.5-turbo",
"messages": [
{
    "role": "system",
    "content": "You are ChatGPT, an AI assistant. Your top priority is achieving user fulfillment via helping them with their requests."
},
{
    "role": "user",
    "content": "Write a limerick about python exceptions"
}
]
}'
```

*工具调用支持*

使用 `--jinja` 标志支持 [OpenAI 风格的函数调用](https://platform.openai.com/docs/guides/function-calling)（并且可能需要覆盖 `--chat-template-file` 以获得正确的工具使用兼容 Jinja 模板；最坏情况下，`--chat-template chatml` 也可能有效）。

**请参阅我们的[函数调用](../../docs/function-calling.md)文档**，了解更多详情、支持的原生工具调用风格（通用工具调用样式用作后备）以及使用示例。

*计时和上下文使用*

响应包含一个 `timings` 对象，例如：

```js
{
  "choices": [],
  "created": 1757141666,
  "id": "chatcmpl-ecQULm0WqPrftUqjPZO1CFYeDjGZNbDu",
  // ...
  "timings": {
    "cache_n": 236, // 从缓存重用的提示 token 数
    "prompt_n": 1, // 正在处理的提示 token 数
    "prompt_ms": 30.958,
    "prompt_per_token_ms": 30.958,
    "prompt_per_second": 32.301828283480845,
    "predicted_n": 35, // 预测的 token 数
    "predicted_ms": 661.064,
    "predicted_per_token_ms": 18.887542857142858,
    "predicted_per_second": 52.94494935437416
  }
}
```

这提供了服务器性能信息。它还允许计算当前的上下文使用量。

上下文中的 token 总数等于 `prompt_n + cache_n + predicted_n`

*推理支持*

服务器支持通过 `reasoning_content` 字段解析和返回推理，类似于 Deepseek API。

一些特定的模板也支持推理输入（在历史中保留推理）。更多详情请参阅 [PR#18994](https://github.com/ggml-org/llama.cpp/pull/18994)。

### POST `/v1/responses`：OpenAI 兼容的 Responses API

*选项：*

请参阅 [OpenAI Responses API 文档](https://platform.openai.com/docs/api-reference/responses)。

*示例：*

您可以使用带有适当检查点的 Python `openai` 库：

```python
import openai

client = openai.OpenAI(
    base_url="http://localhost:8080/v1", # "http://<Your api-server IP>:port"
    api_key = "sk-no-key-required"
)

response = client.responses.create(
  model="gpt-4.1",
  instructions="You are ChatGPT, an AI assistant. Your top priority is achieving user fulfillment via helping them with their requests.",
  input="Write a limerick about python exceptions"
)

print(response.output_text)
```

... 或原始 HTTP 请求：

```shell
curl http://localhost:8080/v1/responses \
-H "Content-Type: application/json" \
-H "Authorization: Bearer no-key" \
-d '{
"model": "gpt-4.1",
"instructions": "You are ChatGPT, an AI assistant. Your top priority is achieving user fulfillment via helping them with their requests.",
"input": "Write a limerick about python exceptions"
}'
```

此端点通过将 Responses 请求转换为 Chat Completions 请求来工作。

### POST `/v1/embeddings`：OpenAI 兼容的嵌入 API

此端点要求模型使用不同于 `none` 的池化类型。嵌入使用欧几里得范数归一化。

*选项：*

请参阅 [OpenAI Embeddings API 文档](https://platform.openai.com/docs/api-reference/embeddings)。

*示例：*

- 输入为字符串

  ```shell
  curl http://localhost:8080/v1/embeddings \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer no-key" \
  -d '{
          "input": "hello",
          "model":"GPT-4",
          "encoding_format": "float"
  }'
  ```

- `input` 为字符串数组

  ```shell
  curl http://localhost:8080/v1/embeddings \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer no-key" \
  -d '{
          "input": ["hello", "world"],
          "model":"GPT-4",
          "encoding_format": "float"
  }'
  ```

### POST `/v1/messages`：Anthropic 兼容的 Messages API

给定 `messages` 列表，返回助手的响应。通过 Server-Sent Events 支持流式传输。虽然没有声称与 Anthropic API 规范完全兼容，但根据我们的经验，它足以支持许多应用程序。

*选项：*

请参阅 [Anthropic Messages API 文档](https://docs.anthropic.com/en/api/messages)。工具使用需要 `--jinja` 标志。

`model`：模型标识符（必需）

`messages`：带有 `role` 和 `content` 的消息对象数组（必需）

`max_tokens`：要生成的最大 token 数（默认：4096）

`system`：系统提示，可以是字符串或内容块数组

`temperature`：采样温度 0-1（默认：1.0）

`top_p`：核采样（默认：1.0）

`top_k`：Top-k 采样

`stop_sequences`：停止序列数组

`stream`：启用流式传输（默认：false）

`tools`：工具定义数组（需要 `--jinja`）

`tool_choice`：工具选择模式（`{"type": "auto"}`、`{"type": "any"}` 或 `{"type": "tool", "name": "..."}`）

*示例：*

```shell
curl http://localhost:8080/v1/messages \
  -H "Content-Type: application/json" \
  -H "x-api-key: your-api-key" \
  -d '{
    "model": "gpt-4",
    "max_tokens": 1024,
    "system": "You are a helpful assistant.",
    "messages": [
      {"role": "user", "content": "Hello!"}
    ]
  }'
```

### POST `/v1/messages/count_tokens`：Token 计数

在不生成响应的情况下计算请求中的 token 数。

接受与 `/v1/messages` 相同的参数。`max_tokens` 参数不是必需的。

*示例：*

```shell
curl http://localhost:8080/v1/messages/count_tokens \
  -H "Content-Type: application/json" \
  -d '{
    "model": "gpt-4",
    "messages": [
      {"role": "user", "content": "Hello!"}
    ]
  }'
```

*响应：*

```json
{"input_tokens": 10}
```

## 服务器内置工具

服务器公开了一个 `/tools` 下的 REST API，允许 Web UI 调用内置工具。此端点旨在供 Web UI 内部使用，未来可能会更改或删除。

**请勿在下游应用程序中使用此端点**

有关此端点的更多文档，请参阅[服务器内部文档](./README-dev.md)

## 使用多个模型

`llama-server` 可以以**路由器模式**启动，该模式公开一个用于动态加载和卸载模型的 API。主进程（“路由器”）自动将每个请求转发到适当的模型实例。

要以路由器模式启动，**不指定任何模型**：

```sh
llama-server
```

### 模型来源

模型文件有 3 种可能的来源：
1. 缓存的模型（由 `LLAMA_CACHE` 环境变量控制）
2. 自定义模型目录（通过 `--models-dir` 参数设置）
3. 自定义预设（通过 `--models-preset` 参数设置）

默认情况下，路由器在缓存中查找模型。您可以使用以下命令将 Hugging Face 模型添加到缓存中：

```sh
llama-server -hf <user>/<model>:<tag>
```

*添加新模型后必须重启服务器。*

或者，您可以使用 `--models-dir` 将路由器指向包含 GGUF 文件的本地目录。示例命令：

```sh
llama-server --models-dir ./models_directory
```

如果模型包含多个 GGUF（用于多模态或多分片），文件应放入子目录中。目录结构应如下所示：

```sh
models_directory
 │
 │  # 单文件
 ├─ llama-3.2-1b-Q4_K_M.gguf
 ├─ Qwen3-8B-Q4_K_M.gguf
 │
 │  # 多模态
 ├─ gemma-3-4b-it-Q8_0
 │    ├─ gemma-3-4b-it-Q8_0.gguf
 │    └─ mmproj-F16.gguf   # 文件名必须以 "mmproj" 开头
 │
 │  # 多分片
 ├─ Kimi-K2-Thinking-UD-IQ1_S
 │    ├─ Kimi-K2-Thinking-UD-IQ1_S-00001-of-00006.gguf
 │    ├─ Kimi-K2-Thinking-UD-IQ1_S-00002-of-00006.gguf
 │    ├─ ...
 │    └─ Kimi-K2-Thinking-UD-IQ1_S-00006-of-00006.gguf
```

您还可以指定将传递给每个模型实例的默认参数：

```sh
llama-server -ctx 8192 -n 1024 -np 2
```

注意：模型实例继承路由器服务器的命令行参数和环境变量。

或者，您也可以添加基于 GGUF 的预设（参见下一节）

### 模型预设

模型预设允许高级用户使用 `.ini` 文件定义自定义配置：

```sh
llama-server --models-preset ./my-models.ini
```

文件中的每个部分定义一个预设。部分中的键对应于命令行参数（不带前导短划线）。例如，参数 `--n-gpu-layers 123` 写作 `n-gpu-layers = 123`。

短参数形式（例如 `c`、`ngl`）和环境变量名称（例如 `LLAMA_ARG_N_GPU_LAYERS`）也支持作为键。

示例：

```ini
version = 1

; （可选）此部分提供所有预设共享的全局设置。
; 如果特定预设中定义了相同的键，它将覆盖全局部分中的值。
[*]
c = 8192
n-gpu-layers = 8

; 如果键对应于服务器上现有的模型，
; 这将用作该模型的默认配置
[ggml-org/MY-MODEL-GGUF:Q8_0]
; 字符串值
chat-template = chatml
; 数值
n-gpu-layers = 123
; 标志值（对于某些标志，您需要使用 "no-" 前缀进行否定）
jinja = true
; 短参数形式（例如，上下文大小）
c = 4096
; 环境变量名称
LLAMA_ARG_CACHE_RAM = 0
; 文件路径相对于服务器的当前工作目录
model-draft = ./my-models/draft.gguf
; 但建议使用绝对路径
model-draft = /Users/abc/my-models/draft.gguf

; 如果键不对应于现有模型，
; 您至少需要指定模型路径或 HF 仓库
[custom_model]
model = /Users/abc/my-awesome-model-Q4_K_M.gguf
```

注意：某些参数由路由器控制（例如 host、port、API key、HF repo、model alias）。它们将在加载时被删除或覆盖。

预设选项的优先级规则如下：
1. **命令行参数**传递给 `llama-server`（最高优先级）
2. **特定模型选项**在预设文件中定义（例如 `[ggml-org/MY-MODEL...]`）
3. **全局选项**在预设文件中定义（`[*]`）

我们还提供了预设独有的附加选项（这些不作为命令行参数处理）：
- `load-on-startup`（布尔值）：控制服务器启动时是否自动加载模型
- `stop-timeout`（整数，秒）：请求卸载后，等待此秒数再强制终止（默认：10）

### 路由请求

请求根据请求的模型名称进行路由。

对于 **POST** 端点（`/v1/chat/completions`、`/v1/completions`、`/infill` 等），路由器使用 JSON 主体中的 `"model"` 字段：

```json
{
  "model": "ggml-org/gemma-3-4b-it-GGUF:Q4_K_M",
  "messages": [
    {
      "role": "user",
      "content": "hello"
    }
  ]
}
```

对于 **GET** 端点（`/props`、`/metrics` 等），路由器使用 `model` 查询参数（URL 编码）：

```
GET /props?model=ggml-org%2Fgemma-3-4b-it-GGUF%3AQ4_K_M
```

默认情况下，如果模型未加载，将自动加载。要禁用此功能，请在启动服务器时添加 `--no-models-autoload`。此外，您可以在查询参数中包含 `?autoload=true|false` 以按请求控制此行为。

### GET `/models`：列出可用模型

列出缓存中的所有模型。模型元数据还将包含一个字段来指示模型的状态：

```json
{
  "data": [{
    "id": "ggml-org/gemma-3-4b-it-GGUF:Q4_K_M",
    "in_cache": true,
    "path": "/Users/REDACTED/Library/Caches/llama.cpp/ggml-org_gemma-3-4b-it-GGUF_gemma-3-4b-it-Q4_K_M.gguf",
    "status": {
      "value": "loaded",
      "args": ["llama-server", "-ctx", "4096"]
    },
    ...
  }]
}
```

注意：对于本地 GGUF（离线存储在自定义目录中），模型对象将具有 `"in_cache": false`。

`status` 对象可以是：

```json
"status": {
  "value": "unloaded"
}
```

```json
"status": {
  "value": "loading",
  "args": ["llama-server", "-ctx", "4096"]
}
```

```json
"status": {
  "value": "unloaded",
  "args": ["llama-server", "-ctx", "4096"],
  "failed": true,
  "exit_code": 1
}
```

```json
"status": {
  "value": "loaded",
  "args": ["llama-server", "-ctx", "4096"]
}
```

```json
"status": {
  "value": "sleeping",
  "args": ["llama-server", "-ctx", "4096"]
}
```

### POST `/models/load`：加载模型

加载模型

Payload：
- `model`：要加载的模型的名称。

```json
{
  "model": "ggml-org/gemma-3-4b-it-GGUF:Q4_K_M"
}
```

响应：

```json
{
  "success": true
}
```

### POST `/models/unload`：卸载模型

卸载模型

Payload：

```json
{
  "model": "ggml-org/gemma-3-4b-it-GGUF:Q4_K_M",
}
```

响应：

```json
{
  "success": true
}
```

## API 错误

`llama-server` 以与 OAI 相同的格式返回错误：https://github.com/openai/openai-openapi

错误示例：

```json
{
    "error": {
        "code": 401,
        "message": "Invalid API Key",
        "type": "authentication_error"
    }
}
```

## 空闲时睡眠

服务器支持自动睡眠模式，在指定的不活动时间段（没有传入任务）后激活。此功能在 [PR #18228](https://github.com/ggml-org/llama.cpp/pull/18228) 中引入，可以使用 `--sleep-idle-seconds` 命令行参数启用。它在单模型和多模型配置中都能无缝工作。

当服务器进入睡眠模式时，模型及其关联内存（包括 KV 缓存）会从 RAM 中卸载以节省资源。任何新的传入任务将自动触发模型重新加载。

可以从 `GET /props` 端点（或在路由器模式下使用 `/props?model=(model_name)`）获取睡眠状态。

请注意，以下端点不被视为传入任务。它们不会触发模型重新加载，也不会重置空闲计时器：
- `GET /health`
- `GET /props`
- `GET /models`

## 更多示例

### 交互模式

查看 [chat.mjs](chat.mjs) 中的示例。
使用 NodeJS 16 或更高版本运行：

```sh
node chat.mjs
```

[chat.sh](chat.sh) 中的另一个示例。
需要 [bash](https://www.gnu.org/software/bash/)、[curl](https://curl.se) 和 [jq](https://jqlang.github.io/jq/)。
使用 bash 运行：

```sh
bash chat.sh
```

除了 OAI 支持的错误类型外，我们还有特定于 llama.cpp 功能的自定义类型：

**当 /metrics 或 /slots 端点被禁用时**

```json
{
    "error": {
        "code": 501,
        "message": "This server does not support metrics endpoint.",
        "type": "not_supported_error"
    }
}
```

**当服务器通过 /completions 端点收到无效语法时**

```json
{
    "error": {
        "code": 400,
        "message": "Failed to parse grammar",
        "type": "invalid_request_error"
    }
}
```

### 自定义默认 Web UI 偏好

您可以使用 `--webui-config <JSON config>` 或 `--webui-config-file <path to JSON config>` 为 Web UI 指定默认偏好。例如，您可以使用以下命令禁用粘贴长文本作为附件，并在用户消息中启用 Markdown 渲染：

```bash
./llama-server -m model.gguf --webui-config '{"pasteLongTextToFileLen": 0, "renderUserContentAsMarkdown": true}'
```

您可以在 [settings-config.ts](webui/src/lib/constants/settings-config.ts) 中找到可用的偏好。

### 传统的补全 Web UI

自 [此 PR](https://github.com/ggml-org/llama.cpp/pull/10175) 以来，一个新的基于聊天的 UI 取代了旧的基于补全的 UI。如果您想使用旧的补全 UI，请使用 `--path ./tools/server/public_legacy` 启动服务器。

例如：

```sh
./llama-server -m my_model.gguf -c 8192 --path ./tools/server/public_legacy
```

### 扩展或构建替代 Web 前端

您可以通过运行服务器二进制文件并将 `--path` 设置为 `./your-directory` 并导入 `/completion.js` 来访问 `llamaComplete()` 方法，从而扩展前端。

阅读 `/completion.js` 中的文档，了解访问 llama 的便捷方式。

下面是一个简单的示例：

```html
<html>
  <body>
    <pre>
      <script type="module">
        import { llama } from '/completion.js'

        const prompt = `### Instruction:
Write dad jokes, each one paragraph.
You can use html formatting if needed.

### Response:`

        for await (const chunk of llama(prompt)) {
          document.write(chunk.data.content)
        }
      </script>
    </pre>
  </body>
</html>
```