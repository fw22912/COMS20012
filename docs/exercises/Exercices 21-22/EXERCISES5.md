# Exercises sheet 5

**Correction around Easter.**

## Fill our survey

<iframe width="640px" height= "480px" src= "https://forms.office.com/Pages/ResponsePage.aspx?id=MH_ksn3NTkql2rGM8aQVG5N9pWWUNd5Khd6GR62JgsZUQjhUWlZOQ1c2V1A5WExWU0hUVjdZMldBSC4u&embed=true" frameborder= "0" marginwidth= "0" marginheight= "0" style= "border: none; max-width:100%; max-height:100vh" allowfullscreen webkitallowfullscreen mozallowfullscreen msallowfullscreen> </iframe>

## A brief history of Operating Systems

1. Look at the photo in first lecture videos. Identify the persons and read about them (check the caption in the slides).

    **Answer:** This won't be in the exam. You can see their name by opening the slides and looking at the caption.
2. Computing was a very feminine profession in its early days. Read about this.

    **Answer:** This won't be in the exam. You can listen to this [podcast](https://www.npr.org/sections/money/2014/10/17/356944145/episode-576-when-women-stopped-coding?t=1617869583003){:target="_blank"} for a quick introduction to the problem.

## OS structure

1. Explain in your own words the concept of interrupt and trap.

    **Answer:** An interrupt is a signal sent to the processor that interrupt (hence the name) the current execution thread. It can be generated by hardware (e.g. memory) or software. A trap is a software generated synchronous interrupts. It may be the result of a fault (e.g. division by zero) or to handle system calls.
2. Explain the concept of rings on Intel architecture. Compare to privilege level on MIPS architecture.

    **Answer:** Intel architecture have 4 Rings (0 to 3). The most privileged ring is Ring 0 which contains the kernel and the most privileged is Ring 3 which contains userspace processes. Ring 1/2 are designed for things such as driver, but are in practice not used by operating systems such as Linux. The Ring restrict the execution of certain type of instructions and memory accesses. MIPS architecture only have two privilege levels.
3. Explain what is a Privilege Level and the Current Privilege Level.

    **Answer:** there is two privileges level in MIPS: kernel or userspace. The privilege level limit the instructions available and memory access (in the sense that some are only accessible to the kernel). The current privilege level is the privilege level of the thread being currently executed.
4. What is a trap handler?

    **Answer:** a trap handle is a special call back function that is invoked when a trap occurs.


## Synchronization primitives

1. What does it mean for an operation to be atomic?

    **Answer:** An operation acting on shared memory is atomic if it completes in a single step relative to other threads.
2. Write pseudocode for the TAS instruction.

    **Answer:** ```bool TestAndSet(bool lock) {
        bool initial = lock;
        lock = true;
        return initial;
    }```
3. Write pseudocode for the CAS instruction.

    **Answer:** ```bool TestAndSet(int* p, int old, int new) {
        if(*p!=old)
            return false;
        *p=new;
        return true;
    }```
4. Why spinlock should only be used when resource will be held for a very short time?

    **Answer:** spinlock is an active lock, the "waiting" thread is busy waiting (i.e. using CPU cycle checking the "lock value"). It is useful when switching context (i.e. picking another thread to execute), would take longer than waiting. If the lock is likely to be held for a long time, it is better to sleep.
5. This problem demonstrates the use of semaphores to coordinate three types of threads: Santa Claus sleeps in his shop at the North Pole and can only be wakened by either

    a. all nine reindeer being back from their vacation in the South Pacific, or

    b. by some of the elves having difficulties making toys.

    To allow Santa to get some sleep, the elves can only wake him when three of them have problems. While three elves are having their problems solved, any other elves wishing to visit Santa must wait for those elves to return. If Santa wakes up to find three elves waiting at his shop's door, along with the last reindeer having come back from the tropics, Santa has decided that the elves can wait until after Christmas, because it is more important to get his sleigh ready. (It is assumed that the reindeer don't want to leave the tropics, and therefore they stay there until the last possible moment.) The last reindeer to arrive must get Santa while the others wait in a warming hut before being harnessed to the sleigh.

		**Answer:** **[BEFORE YOU ASK: THIS WILL NOT BE IN THE EXAM]** This is a classic problem and solutions are widely available on google.
		A workable solution in C:
		```
		#include <pthread.h>
		#include <stdlib.h>
		#include <assert.h>
		#include <unistd.h>
		#include <stdio.h>
		#include <stdbool.h>
		#include <semaphore.h>

		pthread_t *CreateThread(void *(*f)(void *), void *a)
		{
			pthread_t *t = malloc(sizeof(pthread_t));
			assert(t != NULL);
			int ret = pthread_create(t, NULL, f, a);
			assert(ret == 0);
			return t;
		}

		static const int N_ELVES = 10;
		static const int N_REINDEER = 9;

		static int elves;
		static int reindeer;
		static sem_t santaSem;
		static sem_t reindeerSem;
		static sem_t elfTex;
		static sem_t mutex;

		void *SantaClaus(void *arg)
		{
			printf("Santa Claus: Hoho, here I am\n");
			while (true)
			{
				sem_wait(&santaSem);
				sem_wait(&mutex);
				if (reindeer == N_REINDEER)
				{
					printf("Santa Claus: preparing sleigh\n");
					for (int r = 0; r < N_REINDEER; r++)
						sem_post(&reindeerSem);
					printf("Santa Claus: make all kids in the world happy\n");
					reindeer = 0;
				}
				else if (elves == 3)
				{
					printf("Santa Claus: helping elves\n");
				}
				sem_post(&mutex);
			}
			return arg;
		}

		void *Reindeer(void *arg)
		{
			int id = (int)arg;
			printf("This is reindeer %d\n", id);
			while (true)
			{
				sem_wait(&mutex);
				reindeer++;
				if (reindeer == N_REINDEER)
					sem_post(&santaSem);
				sem_post(&mutex);
				sem_wait(&reindeerSem);
				printf("Reindeer %d getting hitched\n", id);
				sleep(20);
			}
			return arg;
		}

		void *Elve(void *arg)
		{
			int id = (int)arg;
			printf("This is elve %d\n", id);
			while (true)
			{
				bool need_help = random() % 100 < 10;
				if (need_help)
				{
					sem_wait(&elfTex);
					sem_wait(&mutex);
					elves++;
					if (elves == 3)
						sem_post(&santaSem);
					else
						sem_post(&elfTex);
					sem_post(&mutex);

					printf("Elve %d will get help from Santa Claus\n", id);
					sleep(10);

					sem_wait(&mutex);
					elves--;
					if (elves == 0)
						sem_post(&elfTex);
					sem_post(&mutex);
				}
				// Do some work
				printf("Elve %d at work\n", id);
				sleep(2 + random() % 5);
			}
			return arg;
		}

		int main(int ac, char **av)
		{
			elves = 0;
			reindeer = 0;
			sem_init(&santaSem, 0, 0);
			sem_init(&reindeerSem, 0, 0);
			sem_init(&elfTex, 0, 1);
			sem_init(&mutex, 0, 1);

			pthread_t *santa_claus = CreateThread(SantaClaus, 0);

			pthread_t *reindeers[N_REINDEER];
			for (int r = 0; r < N_REINDEER; r++)
				reindeers[r] = CreateThread(Reindeer, (void *)r + 1);

			pthread_t *elves[N_ELVES];
			for (int e = 0; e < N_ELVES; e++)
				elves[e] = CreateThread(Elve, (void *)e + 1);

			int ret = pthread_join(*santa_claus, NULL);
			assert(ret == 0);
		}
		```
