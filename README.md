此文件用于为astrbot中的语音和文字交替输出，默认触发bot语音的概率为30%，文本为70%。
经过测试，该文件可以跳过tts生成语音，可以帮助用户减少费用。
如果需要修改概率，请在您替换过的 respond/stage.py 文件中，找到以下代码段：

Python

            # 2. 如果消息链同时包含文字和语音，执行随机逻辑
            if has_plain and has_record:
                # random.random() < 0.7 表示 70% 概率只发文字
                if random.random() < 0.7:
                    # 30% 概率：只发送文字 (移除所有 Record 语音组件)
                    logger.info("随机选择：仅发送文本 (移除语音组件)")
                    result.chain = [
                        comp
                        for comp in result.chain
                        if not isinstance(comp, Comp.Record)
                    ]
                else:
                    # 30% 概率：只发送语音 (移除所有 Plain 文本组件)
                    logger.info("随机选择：仅发送语音 (移除文本组件)")
                    result.chain = [
                        comp
                        for comp in result.chain
                        if not isinstance(comp, Comp.Plain)
                    ]
如果您想将概率改为 40% 文字 / 60% 语音，您需要将代码修改为：

Python

            # 2. 如果消息链同时包含文字和语音，执行随机逻辑
            if has_plain and has_record:
                # random.random() < 0.4 表示 40% 概率只发文字
                if random.random() < 0.4:  # <-- 将 0.3 改为 0.4
                    # 40% 概率：只发送文字 (移除所有 Record 语音组件)
                    logger.info("随机选择：仅发送文本 (移除语音组件)")
                    # ... (其余代码保持不变) ...
                else:
                    # 60% 概率：只发送语音 (移除所有 Plain 文本组件)
                    logger.info("随机选择：仅发送语音 (移除文本组件)")
                    # ... (其余代码保持不变) ...
修改完成后，请务必保存文件并重启 AstrBot (uv run main.py)，新的概率就会生效
