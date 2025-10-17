此文件用于为astrbot中的语音和文字交替输出，默认触发bot语音的概率为30%，文本为70%。
经过测试，该文件可以跳过tts生成语音，可以帮助用户减少费用。
如果需要修改概率，请在您替换过的 respond/stage.py 文件中，找到以下代码段：

请打开文件 astrbot/core/pipeline/result_decorate/stage.py，找到以下代码块：
Python            # --- 【开始插入】随机选择逻辑 (30% 文本 / 70% 语音) --------------------------------
            should_generate_tts = True
            
            # 仅对 LLM 结果应用随机逻辑
            if result.is_llm_result() and self.ctx.astrbot_config["provider_tts_settings"]["enable"]:
                
                # 随机选择概率 (0.3 for text, 0.7 for voice)
                if random.random() < 0.3: # <-- 就是这一行！
                    should_generate_tts = False
                    logger.info("随机选择：30% 仅发送文本。跳过 TTS 生成，**节省成本**。")
                else: 
                    logger.info("随机选择：70% 仅发送语音。将进行 TTS 生成，并移除 Plain 组件。")
            
            # --- 【结束插入】随机选择逻辑 --------------------------------
修改完成后，请保存文件并重启 AstrBot (uv run main.py) 使其生效。
