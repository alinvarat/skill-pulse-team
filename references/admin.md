# คู่มือผู้ดูแล pulse-team

- ใช้ `hermes -p <profile> config set KEY VALUE` เปลี่ยน config ไม่เขียน YAML ด้วยมือ; เก็บ model, token, owner/channel gates เดิม (ตั้ง `fallback_providers` ให้ครบทั้ง 11 โปรไฟล์และตรวจด้วย `hermes -p <profile> fallback list` เพื่อกันสตรีมหลุดแบบไม่มีตัวสำรอง)
- บน Discord หากเมนู slash command error ให้พิมพ์ `/new` หรือ `/delegation profile_routes --now` เป็นข้อความธรรมดาแทน
- ติดตั้ง plugin เฉพาะ Chief ผ่าน `hermes -p pulse-general plugins install advanced-delegation --enable`; target ไม่ต้องติดตั้ง plugin
- ตั้ง `delegation.backends.profile_routes.profiles` เป็นรายการ `{name, description}` และ `delegation.backend=profile_routes`; จำกัด `max_concurrent_children=3`, `max_spawn_depth=1`; ไม่มี self route
- ตรวจ dynamic schema จาก registry หลัง plugin discovery และทำ bounded real delegation ก่อนอ้างว่าใช้ได้ การ parse config อย่างเดียวไม่พิสูจน์ inference/Discord end-to-end
- One-shot `hermes -z` มี `delegation.oneshot_max_children` แยกจาก concurrency (เครื่องนี้ค่าเริ่มต้น 2): ทดสอบสาม specialist เป็น batch 2 + อีก invocation 1 ไม่เพิ่มสิทธิ์/งบถาวรเพียงเพื่อเทสต์; exit code 0 ของ CLI อาจยังตอบ tool error ต้องตรวจ tool responses/transcripts จริงด้วย
- Plugin รองรับ hot reload แต่ tools/SOUL ของเซสชันเก่าอาจ pinned; ใช้ `/new` หรือวิธี refresh ที่เอกสารระบุ และรีสตาร์ท host gateway (`-p default` แบบ detached background หลัง `sleep` สั้น ๆ) เมื่อแก้โค้ด/ปลั๊กอินที่ต้องโหลดใหม่
- เอกสารอ้างอิง: https://hermes-agent.nousresearch.com/docs/user-guide/features/delegation และ official plugin README https://github.com/NousResearch/hermes-official-plugins/tree/main/advanced-delegation
