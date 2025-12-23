# changes for `maman02`

---

## File: maman02/xv6-02/Makefile

```diff
@@ -214,6 +214,7 @@ UPROGS=\
         _demo_pid_ns \
         _demo_mount_ns \
         _ioctltests \
+	_ps\
 
 INTERNAL_DEV=\
 	internal_fs_a\
@@ -224,7 +225,7 @@ internal_fs_%: mkfs
 	dd if=/dev/zero of=$@ count=80
 	./mkfs $@ 1
 
-fs.img: mkfs README $(INTERNAL_DEV) _init #$(UPROGS) 
+fs.img: mkfs README $(INTERNAL_DEV) _init _ps #$(UPROGS) 
 	./mkfs fs.img 0 README $(UPROGS) $(INTERNAL_DEV)
 
 -include *.d
```

---

## File: maman02/xv6-02/defs.h

```diff
@@ -181,6 +181,7 @@ int             wait(int*);
 void            wakeup(void*);
 void            yield(void);
 int             cgroup_move_proc(struct cgroup * cgroup, int pid);
+int             cps108(void);
 
 // swtch.S
 void            swtch(struct context**, struct context*);
```

---

## File: maman02/xv6-02/proc.c

```diff
@@ -1,4 +1,4 @@
-#include "types.h"
+ #include "types.h"
 #include "defs.h"
 #include "param.h"
 #include "memlayout.h"
@@ -839,3 +839,44 @@ struct cgroup *proc_get_cgroup(void)
      cg = proc->cgroup;
   return cg;
 }
+
+//current process status
+int
+cps108()
+{
+  struct proc *p;
+
+  acquire(&ptable.lock);
+
+  cprintf("name \t pid \t state \t \t extpid \t ppid \t size \t cpu_time\n");
+  for(p = ptable.proc; p < &ptable.proc[NPROC]; p++) {
+
+    if(p->state == UNUSED)
+      continue;
+
+    char *state;
+    switch(p->state) {
+    case RUNNING:
+      state = "RUNNING"; 
+      break;
+    default:
+      state = "SLEEPING";
+      break;
+    }
+
+    int extpid = p->pids[1].pid;
+    if(extpid == 0)
+      extpid = p->pids[0].pid;
+    
+    int ppid = 0;
+    if(p->parent)
+      ppid = p->parent->ns_pid;
+
+    cprintf("%s \t %d \t %s \t \t %d \t %d \t %d \t %d \n",
+            p->name, p->ns_pid, state, extpid, ppid, p->sz, p->cpu_time);
+  }
+
+  release(&ptable.lock);
+
+  return 33;
+}
\ No newline at end of file
```

---

## File: maman02/xv6-02/runtests.exp

```diff
old mode 100644
new mode 100755
```

---

## File: maman02/xv6-02/syscall.c

```diff
@@ -114,6 +114,7 @@ extern int sys_getppid(void);
 extern int sys_getcpu(void);
 extern int sys_getmem(void);
 extern int sys_kmemtest(void);
+extern int sys_cps108(void);
 
 static int (*syscalls[])(void) = {
 [SYS_fork]    sys_fork,
@@ -148,6 +149,7 @@ static int (*syscalls[])(void) = {
 [SYS_getcpu] sys_getcpu,
 [SYS_getmem] sys_getmem,
 [SYS_kmemtest] sys_kmemtest,
+[SYS_cps108]  sys_cps108,
 };
 
 void
```

---

## File: maman02/xv6-02/syscall.h

```diff
@@ -31,3 +31,4 @@
 #define SYS_getcpu 30
 #define SYS_getmem 31
 #define SYS_kmemtest 32
+#define SYS_cps108 33
```

---

## File: maman02/xv6-02/sysproc.c

```diff
@@ -253,3 +253,8 @@ int
 sys_kmemtest(void) {
   return kmemtest();
 }
+
+int
+sys_cps108(void) {
+  return cps108();
+}
\ No newline at end of file
```

---

## File: maman02/xv6-02/user.h

```diff
@@ -32,6 +32,7 @@ char* sbrk(int);
 int sleep(int);
 int usleep(unsigned int);
 int uptime(void);
+int cps108(void);
 int ioctl(int fd, unsigned long request, ...);
 int getppid(void);
 int getcpu(void);
```

---

## File: maman02/xv6-02/usys.S

```diff
@@ -40,3 +40,4 @@ SYSCALL(getppid)
 SYSCALL(getcpu)
 SYSCALL(getmem)
 SYSCALL(kmemtest)
+SYSCALL(cps108)
```

